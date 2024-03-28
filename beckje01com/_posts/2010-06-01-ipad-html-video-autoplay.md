---
author: Jeff Beck
title: iPad HTML Video Autoplay
excerpt:
categories:
  - Web Development
tags:
  - HTML5
  - iPad
  - iPhone
  - Mobile
post_format: [ ]
comments: true
date: 2010-06-01T16:40:52+0000
---
**The Problem**

The iPad and also the iPhone have a bit of special behavior when it comes to using the HTML5 video tag. They both have the autoplay functionality shut off. This behavior is also extended to any event which the user has not started the event chain. (AKA you can’t do the following)


``` html
<body onLoad="document.myMovie.play()">
```
**What it Means**

So what this really means you can not have a playlist of videos where one video is loaded as soon as the first ends. Also having timed events from one video kick off sub videos will not work unless the user clicks on the new video.

**Is this a Bug?**

No this is as intended by Apple look for the Device-Specific Considerations [here][1]. What the idea is that any device that could be charged from a data plan shouldn’t allow auto play. While this idea makes sense the execution is poor. I am currently using a wifi only iPad so there is no reason for this restriction it would be best in my mind to detect if they are using wifi or 3g and react accordingly.

**What to Do**  

Design around this I restructured an interaction from popping up this sub video over the main video to instead make a thumbnail active so it could be click to show this sub video this works in my case as the sub video can be viewed any time after the timed event.

Another option is to show the new video element that will display a play symbol you should be able to set a poster image with some text to guide the users.

 [1]: http://developer.apple.com/safari/library/documentation/AudioVideo/Conceptual/Using_HTML5_Audio_Video/AudioandVideoTagBasics/AudioandVideoTagBasics.html
