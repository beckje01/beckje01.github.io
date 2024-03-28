---
author: Jeff
title: >
  ExpressionEngine Pagination and the
  Dynamic Parameter
excerpt:
layout: post
categories:
  - Web Development
tags:
  - ExpressionEngine
  - PHP
post_format: [ ]
comments: true
date: 2009-02-26T19:09:22+0000
---
While working with an ExpressionEngine based site I ran across a listing of a weblog that could contain 1000′s of posts yet as it was displayed with the Dynamic Parameter off there was no stock way to support Pagination of the items. I used a simple hack, shown at the end of the post, that allowed me to use the pagination but it shows a bigger issue with ExpresionEngine, while it may not come up that often there still should be pagination for any `<font face="sans-serif">exp:weblog:entries set but it is not. There is no reason that non-dyanmic listings shouldn't be paginated if anything it would be more likely needed as they are likely to be a long list of a whole set.<br /><br />The hack I used to enable pagination while the dynamic="off" was available <a target="_blank" href="http://expressionengine.com/wiki/Enable_pagination_with_dynamic_off/">here</a> on the ExpressionEngine Wiki but as it is a wiki I felt that I should give you the basics here as well.</p>
<p>In </font>`system/modules/weblog/mod.weblog.php find the comment “Parse page number” you need to simply remove the AND $dynamic from the if statment to allow for the pagination use inside a non-dynamic weblog listing.

Start with:  

```php
    if (preg_match("#^P(\d+)|/P(\d+)#", $qstring, $match) AND $dynamic)
    {
    ...
```
Change to:  

```php
if (preg_match("#^P(\d+)|/P(\d+)#", $qstring, $match) )
    {
    ...

```
