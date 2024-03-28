---
author: Jeff
title: Read only tables with IE6/IE7 Workaround
excerpt:
layout: post
categories:
  - JavaScript
  - Web Development
tags:
  - IE6
  - IE7
  - JavaScript
  - Web Development
post_format: [ ]
comments: true
date: 2008-11-05T09:16:36+0000
---
In an earlier post, I reported that the TR element in IE6/IE7 had a read only innerhtml. I have found a way to get around this while not perfect it works. It turns out while the TR’s innnerhtml is read only a TD is not. So you can add a TD with a column span equal to the total column in the table. Then you can add what you wanted to the TD’s innerhtml. Check out the example code below.  

```js
function addrow(http_request,elm)
    {
            var temp= http_request;
            var tablebody = elm.tBodies[];
              try
              {
                var row = tablebody.insertRow(elm.rows.length-1);
                var cell = row.insertCell(-1);
                cell.colSpan = "6";
                cell.className="nopadding";
                cell.innerHTML = "<table style='margin:0; padding:0; width:100%' border='1'><tr>"+temp+"</tr></table>";
              } catch (e)
              {
                var row = tablebody.insertRow(elm.rows.length-1);
                row.id ="sctablerow"+nextelm.value;
                row.innerHTML = temp;
              }
    }
```
