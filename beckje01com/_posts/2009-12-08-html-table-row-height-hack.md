---
author: Jeff Beck
title: HTML Table Row Height Hack
excerpt:
categories:
  - Web Development
tags:
  - HTML
  - Web Development
post_format: [ ]
comments: true
date: 2009-12-08T15:24:24+0000
---
Working with a fixed height and width table I found a problem where rows were expanding too much I needed each row to only grow in height enough to contain the text and not fill the rest of the table. The number of rows was dynamic but the height fixed. The hack is simple add a height to each row of 1px which will make it so the rows only grow enough to contain your text then add a last row to act as a buffer with no height. Below the fold I have examples.

``` html
    <table height="200px">
        <tr height="1">
            <td><img src="example.gif"/></td>
            <td>Some Text</td>
        </tr>
        <tr height="1">
            <td><img src="example2.gif"/></td>
            <td>Some More Text</td>
        </tr>
        <tr><td colspan="2"/></tr> <!--The Buffer Row-->
    </table>

```
