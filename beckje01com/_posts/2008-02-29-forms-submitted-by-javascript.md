---
author: Jeff
title: 'Forms Submitted by JavaScript '
excerpt:
layout: post
categories:
  - JavaScript
  - Web Development
tags:
  - HTML Forms
  - JavaScript
  - Web Development
post_format: [ ]
comments: true
date: 2008-02-29T00:45:09+0000
---
While building a simple shopping cart I was adding an update link so customers could easily update the cart quantities I would have done Ajax but we are on a time crunch and it would be more billable so get this much working now add the rest in the second phase. So I have my normal submit button, called submit and I wanted a simple link that said update that would submit the form to another page. Well thats not hard so I simply changed the action of the form and called the submit function, as seen below.

``` js
document.getElementById("cartform").action = 'cart.php?action=update';
      document.mycart.submit();
```

Well that should work fine, oh but no it doesn’t nothing at all was happening it was very odd. So I got out the error console and find that submit is not a function. Well thats odd. So I assume that I have and outdated reference for JavaScript so I dig around and all the example I see work just fine. After staring at it for what seemed hours I thought it better not be over writing the submit function for the form with the button named submit. Well it was, SOB. So if you make sure to not overwrite functions of forms with your field names, its just a big headache.
