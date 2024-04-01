---
author: Jeff Beck
title: HTML5 Video Poster and the iPad
excerpt:
categories:
  - Uncategorized
  - Web Development
tags:
  - HTML5
  - iPad
  - Mobile
  - video
  - Web Development
post_format: [ ]
comments: true
date: 2010-04-17T07:44:18+0000
---
While working on a small HTML5 based project for the iPad I found some interesting things with how it handles the poster attribute and defining the video source. The following HTML works fine on Mobile Safari (iPad) and my desktop Chrome.

``` html
      <video id="vidtest" src="big_buck_bunny.mp4"  poster='poster.jpg' controls="controls">
      </video>
```
But moving to the following code works fine in Chrome but on the iPad (Mobile Safari) it will only show the poster and no way to start the video unless you do the pinch zoom to make the video full screen then the controls will show up.

``` html
       <video id="vidtest"  poster='poster.jpg' controls="controls" >
          <source src="big_buck_bunny.mp4" type="video/mp4" />
       </video>
```

Simply using the first style is ok if you are only doing one type of video but if you wanted to include other options this issue limits that ability. But for now if you are targeting iPad just use the first option.  
