---
author: Jeff
title: >
  Silverlight Move Cursor to End of
  TextBox
excerpt:
layout: post
categories:
  - 'C#'
  - Silverlight
  - Web Development
tags: [ ]
post_format: [ ]
comments: true
date: 2009-04-04T20:46:03+0000
---
While working on in Silverlight I found the need to auto populate a TextBox then shift focus to that TextBox. That was simple to do with the following code.


``` c#
    txtSearch.Focus();
    txtSearch.Text = "sometext ";
```

But then there is a problem the cursor is at the start of the text box so if a user starts typing they end up adding to the start when I want them to just add on to the end. So after some hunting I found the SelectionStart property of the TextBox, while this may be obvious to people who work in Silverlight and C# all the time it didn’t jump right out at me so I thought it was worth talking about. So by using SelectionStart and I can move the cursor to the end and users can just append to my started search text.

``` c#
    txtSearch.Focus();
    txtSearch.Text = "sometext ";
    txtSearch.SelectionStart = txtSearch.Text.Length;
```
