---
author: Jeff Beck
title: HTML5 Video and the seeked event
excerpt:
categories:
  - JavaScript
  - Web Development
tags:
  - HTML5
  - JavaScript
  - video
  - Web Development
post_format: [ ]
comments: true
date: 2010-05-04T03:18:00+0000
---
I have been working with the HTML5 video element a bit on both iPad and the Chrome browser. I’ve started to find some odd behavior around the seeked event. First a touch of background about the seeked event. According to the [spec][1] the seeked event should fire when the seeking IDL attribute changes to false. This seems to make perfect sense but when actually using the event it seems inconsistent at best.

I have seen three distinct actions that don’t make sense to me.  


## Extra Fires

*   While dragging the scrub bar the seeked event will fire many times in the Google Chrome 5.0.342.9 beta on windows.
*   When setting the position by changing the currentTime attribute it will fire the seeked event three times in a row on both the iPad and Chrome.

## No Fires

*   Scrubbing on the iPad never fires the seeked event.

So from what I can tell its not the best idea to use the seeked event at this point but the timeupdate event does fire consistently through my testing. So what you can do if you like is keep track of a last value for the timeupdate and then you can detect if it jumps too far (up to you and your use) then fire the logic you wanted to fire on seeked. I am kicking off events at times and I didn’t want to miss events so simply changing my logic to fire as long as the time is past the event point worked fine for my uses but that may not always work. Keeping track of the last time from the timeupdate shouldn’t be a very hard addition if you need special seek logic.

 [1]: http://www.whatwg.org/specs/web-apps/current-work/multipage/video.html#mediaevents
