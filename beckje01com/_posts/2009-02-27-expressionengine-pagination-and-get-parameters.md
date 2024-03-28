---
author: Jeff
title: >
  ExpressionEngine Pagination and Get
  Parameters
excerpt:
layout: post
categories:
  - Web Development
tags:
  - ExpressionEngine
  - PHP
post_format: [ ]
comments: true
date: 2009-02-27T00:55:58+0000
---
This may not come up that often but if you are using Get parameters such as lets say submitted by a form to filter some weblog entries, these parameters will not be preserved while using the built in pagination. This can be a real issue if you are trying to filter results and let users page through them but we can add a touch of PHP and fix this issue. In your paginate tags we just need to add the query string to the page link. You’ll notice in the sample we use the server parameter ‘query_string’ along with the question mark which will produce our correct links.


``` php
    {paginate}
      {if previous_page}
        <a href="{auto_path}<?php echo "?".$_SERVER['QUERY_STRING']; ?>" class='paginate-previous'>Previous</a>
      {/if}

      {if next_page}
        <a href="{auto_path}<?php echo "?".$_SERVER['QUERY_STRING']; ?>" class='paginate-next'>Next</a>
      {/if}
    {/paginate}
```
