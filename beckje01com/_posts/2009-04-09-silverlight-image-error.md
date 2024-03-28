---
author: Jeff
title: Silverlight Image Error
excerpt:
layout: post
categories:
  - Silverlight
tags: [ ]
post_format: [ ]
comments: true
date: 2009-04-09T23:26:16+0000
---
While working in Silverlight I ran across a error that seemed rather odd, not because it didn’t make sense but that I couldn’t see anything I did wrong. So the error was

Unhandled Error in Silverlight 2 Application App.xap  
Code: 4001  
categories: ImageError  
Message: AG\_E\_NETWORK_ERROR

While checking the Image tag in the xaml I simply couldn’t find anything wrong with it. It looked very similar to the following xaml. The user.profile_image was of course a full URI that worked fine when written directly in the browser.


``` html
<Image  Grid.Row="0" Margin="0,0,5,0" VerticalAlignment="Top"  Source="{Binding user.profile_image}" ></Image>
```

So that leaves me with what could the problem be, well it turned out it was simple I was a using a GIF file so http://mydomain.com/foo.gif as a source for an image simply doesn’t work as GIF isn’t supported. But really why would Silverlight not use GIF? Well simply converting the GIF to a PNG then Silverlight can handle the image just fine and display it. Although there are some issues as in Silverlight the only 8 bit and fewer PNG formats support transparency. So if the reason you were using a GIF was for transparency you will have to create an 8 bit or lower PNG. Look for a simple how to next post on converting a GIF to a PNG that Silverlight can display properly with transparency.
