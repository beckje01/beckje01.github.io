---
author: Jeff Beck
title: >
  JavaScript Passing by Value into an
  Inline Function
excerpt:
categories:
  - JavaScript
  - Web Development
tags:
  - Ajax
  - Java
  - JS
  - Web Development
post_format: [ ]
comments: true
date: 2009-12-31T11:49:05+0000
---
I have a web page that needs to go through a bootstrap step in order to make all the correct Ajax calls. So I was simply looping through the results and making the Ajax call like the following. You will note that I needed to pass a name to the processing of the data returned but my first attempt was not working.

``` js
    for(i=;i<myJSONObject.campus.length;i++)
    {
      campus_name = myJSONObject.campus[i].name;
      var campURL=myJSONObject.campus[i].url;
      $.ajax({
            type:"post",
            url: campURL+"/fetchcampusdata.jsp?campusid="+id,
    	datatype:"JSON",
    	cache: false,
    	success: function(data2)
    	{
    		processCampusData(data2, campus_name);
    	},
    	error: function(request,error)
            {
    		alert("error in processing campus data: "+error) ;
    	}
      });

    }
```

That code was not working correctly as all the names were coming up as the last one in the loop. And thinking more about it that makes perfect sense. The problem is when the success function fires it looks back to the Activation Record which contains campus\_name which has been modified by the loop. So what I need to do to fix it was simply move the Ajax call to a function so an AR would be created for each call allowing me to use the campus_name variable correctly.

``` js
    for(i=;i<myJSONObject.campus.length;i++)
    {
      campus_name = myJSONObject.campus[i].name;
      var campURL=myJSONObject.campus[i].url;
      callFetch(campURL[],myJSONObject.campus[i].id,campus_name);
    }

    function callFetch(url,id,campus_name)
    {
      $.ajax({
            type:"post",
            url: url+"/fetchcampusdata.jsp?campusid="+id,
    	datatype:"JSON",
    	cache: false,
    	success: function(data2)
    	{
    		processCampusData(data2, campus_name);
    	},
    	error: function(request,error)
            {
    		alert("error in processing campus data: "+error) ;
    	}
      });
    }
```
