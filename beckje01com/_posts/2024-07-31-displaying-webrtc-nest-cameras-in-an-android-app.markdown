---
author: Jeff Beck
title: Displaying WebRTC Nest Cameras in an Android App
date: 2024-07-31T19:39:56+00:00
comments: true
categories:
- Android
- WebRTC
tags:
- Nest
- WebRTC
- Android
---

Wanting to display some Nest Cameras in an Android App, I needed to deal with the fact that there were two primary flavors of cameras. In particular, the version I needed to support first was the Wired Camera that exposed WebRTC-based video streams. So, in order to accomplish showing WebRTC in an Android app, I took the approach of embedding a WebView and depending on the native JavaScript support for WebRTC.

<!--more-->

## Parts

- [API Access](#api-access)
- [HTML Page](#html-page)
- [JavaScript](#javascript)
- [Embed into Android](#embed-into-android)

### API Access

To use the Device APIs Google offers for the Nest devices, you need to sign up and go through the basic setup steps. You will have to pay a fee of $5 as part of this setup. But after that, you use standard OAuth 2 flows to get your token. 

The developer docs for this are great, and I can’t add much other than to point you to the following them:

[Nest Device Access Docs](https://developers.google.com/nest/device-access/registration)

### HTML Page

We don't need much for the HTML page: 

- `video` tag - Our stream will be shown here.
- `script` tag - Load our JavaScript that will connect to the Nest Camera

```html
<html>
    <head>
        <link rel="stylesheet" href="/assets/stylesheet.css">
    </head>
    <body>
        <video id="htmlVideo" playsinline autoplay muted></video>
        <script src="/assets/video.js"></script>
    </body>
</html>
```

The `video` tag has a few extra attributes added: `playsinline`, `autoplay`, `muted`. These three attributes are needed in order to allow the JavaScript to start the feed when the page is loaded automatically. Without those three attributes, you will likely need a user action of pressing play or some click to start the stream.  


### JavaScript

The JavaScript doing all the real work is taken from [James Dilworth's](https://jamesdilworth.com/) post [Stream from the New Nest Camera (Battery) to the Web](https://jamesdilworth.com/all/stream-from-new-nest-camera-to-the-web/). Some minor bits were removed and streamlined, but this is the same as the post, so refer to the primary source for any information.

```js
const configuration = { };

let peerConnection = null;
let remoteStream = null;

async function startFeed() {

  peerConnection = new RTCPeerConnection();
  const token = "<token>"; // Change This

  // Prep remote stream
  remoteStream = new MediaStream();


  // Watch for new tracks from remote stream; add to video stream
  peerConnection.ontrack = event => {
    event.streams[0].getTracks().forEach(track => {
        remoteStream.addTrack(track);
    });
  };

  // Data Channel Required by the SDM API.
  peerConnection.createDataChannel("dataSendChannel");


  // Create the offer.
  const offer = await peerConnection.createOffer({
     offerToReceiveAudio: true,
     offerToReceiveVideo: true
  });
  peerConnection.addTransceiver('audio', { direction: 'recvonly' });
  await peerConnection.setLocalDescription(offer);

  // Now, send request for SDP connection
  const url = 'https://smartdevicemanagement.googleapis.com/v1/enterprises/<project_id>/devices/<deivce_id>:executeCommand';
  const options = {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': 'Bearer ' + token
    },
    body: JSON.stringify({
      "command": "sdm.devices.commands.CameraLiveStream.GenerateWebRtcStream",
      "params" : {
        "offerSdp" : offer.sdp
      }
    })
  };

  const response = await fetch(url, options);

  // Process the response.
  const data = await response.json();
  let answerSdp = data.results.answerSdp;

  // Now start the feed using the answer
  const answer = new RTCSessionDescription({type: 'answer', sdp: answerSdp});
  await peerConnection.setRemoteDescription(answer);

  // This selector matchs the id on the video element in the HTML
  document.querySelector('#htmlVideo').srcObject = remoteStream;
  // The video should now be running.
}

startFeed();
```

You'll note that we are selecting the `video` tag by id and updating the `srcObject` that will actually start displaying the stream from the camera.


### Embed into Android

To actually show the HTML inside our Android app, we are going to make use of [Loading in-app Content](https://developer.android.com/develop/ui/views/layout/webapps/load-local-content#java).  The general idea is to set up a `WebViewAssetLoader` that will know how to for certain paths load files local to the app. We also need a WebView.


First, we set up the `WebViewClientCompat` subclass just like the tutorial linked.

``` java
private static class LocalContentWebViewClient extends WebViewClientCompat {

    private final WebViewAssetLoader mAssetLoader;

    LocalContentWebViewClient(WebViewAssetLoader assetLoader) {
        mAssetLoader = assetLoader;
    }

    @Override
    @RequiresApi(21)
    public WebResourceResponse shouldInterceptRequest(WebView view,
                                     WebResourceRequest request) {
        return mAssetLoader.shouldInterceptRequest(request.getUrl());
    }

    @Override
    @SuppressWarnings("deprecation") // To support API < 21.
    public WebResourceResponse shouldInterceptRequest(WebView view,
                                     String url) {
        return mAssetLoader.shouldInterceptRequest(Uri.parse(url));
    }
}
```

Next, we need a WebView. I used the following inside a fragment XML.

``` xml
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="20dp">

    <WebView
        android:id="@+id/web_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</LinearLayout>
```

For this app, I wanted to setup loading the HTML and JS we have from earlier after the view is created.

``` java
@Override
public void onViewCreated(@NonNull View view, @Nullable Bundle savedInstanceState) {

    //Configure the Asset Loader to for our known paths use local assets and resources.
    WebViewAssetLoader webViewAssetLoader = new WebViewAssetLoader.Builder()
            .setDomain("beckje01.io") //Set a domain we are going to assume is in all the HTML / JS
            .addPathHandler("/assets/", //Load assets for this path.
                            new WebViewAssetLoader.AssetsPathHandler(this.getContext()))  
            .addPathHandler("/res/", //Load resources for this path.
                            new WebViewAssetLoader.ResourcesPathHandler(this.getContext()))  
            .build();

    //Find WebView from the fragment.
    WebView wb = view.findViewById(R.id.web_view);

    if (wb != null) {
        //Set WebViewClient to the Class we created before.
        wb.setWebViewClient(new LocalContentWebViewClient(webViewAssetLoader));

        //We need JavaScript enabled for our WebRTC video to actually work.
        wb.getSettings().setJavaScriptEnabled(true);

        //Load the apps local asset index.thml
        wb.loadUrl("https://beckje01.io/assets/index.html");

    } else {
        //TODO Handle Missing WebView
    }
    super.onViewCreated(view, savedInstanceState);
}
```
With all that wired together, you should now have a WebRTC-based camera stream working inside a WebView in your Android app.

## Notes

- WebRTC would only decode correctly for me with a real android device not an emulated one.
- This was needed for a project that didn't need to be certified by Nest or Google in anyway I can't speak to if these methods would pass those checks.


## Refrences

- Web example streaming with WebRTC [Stream from the New Nest Camera (Battery) to the Web](https://jamesdilworth.com/all/stream-from-new-nest-camera-to-the-web/)
- [Nest Camera API Docs](https://developers.google.com/nest/device-access/api/camera-wired)
- [Nest Device Access Docs](https://developers.google.com/nest/device-access/registration)