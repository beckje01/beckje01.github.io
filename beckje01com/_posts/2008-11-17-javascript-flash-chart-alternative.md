---
author: Jeff
title: JavaScript / Flash Chart Alternative
excerpt:
layout: post
categories:
  - JavaScript
  - Web Development
tags:
  - JavaScript
  - Web Development
post_format: [ ]
comments: true
date: 2008-11-17T22:30:08+0000
---
So many times it is nice to add a dynamic graph to a web page. I’ve personally done this with JavaScript libraries like [PlotKit ][1]or [Plotr][2]. Also there is the use of Flash with PHP or ASP.Net in [amCharts][3]. But if the user doesn’t have Flash or JavaScript turned off these tools fail. While this doesn’t happen too often any more it still may be an issue. But one issue I’ve noticed is that these charts do take a lot of resources to run so if you have visitors with slower computers that may be a real issue.

So we need a simple alternative, such as generating an image server side and serving that up. While this isn’t too difficult you may not have an imaging library available or the resources to spare. That is where [Google Charts][4] comes in with their simple API you can quickly add graphs to your web page. It’s really simple just embed a special URL in an image tag which you can easily make dynamic with PHP, ASP, or any dynamic system you’re using. See the example below. It works very well, with minimal setup and no added software for clients is needed.  

<!-- more -->

```html
<img src="http://chart.apis.google.com/chart?
    chs=250x100
    &amp;chd=t:60,40
    &amp;cht=p3
    &amp;chl=Hello|World"
    alt="Sample chart" />
```
![Sample chart][5]

 [1]: http://www.liquidx.net/plotkit/
 [2]: http://solutoire.com/plotr/
 [3]: http://www.amcharts.com/
 [4]: http://code.google.com/apis/chart/
 [5]: http://chart.apis.google.com/chart?chs=250x100&chd=t:60,40&cht=p3&chl=Hello|World
