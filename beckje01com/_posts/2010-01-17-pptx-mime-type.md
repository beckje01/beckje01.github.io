---
author: Jeff Beck
title: pptx mime type
excerpt:
categories:
  - Web Development
tags:
  - mime
  - Web Development
post_format: [ ]
comments: true
date: 2010-01-17T10:48:16+0000
---
I was working with a CMS and found that one file was not downloading correctly people were saying that the file was being zipped up by the CMS. Well it turns out it was the issue with IE6 and a bad MIME Type. The fix is simple for pptx you need the mime type *application/vnd.openxmlformats-officedocument.presentationml.presentation* once that was added everything was working correctly. With out this a PPTX file will download as a zip.

A few links on the subject:

*   [Office 2007 Mime Types][1]
*   [How to fix in Apache][2]

 [1]: http://blogs.msdn.com/dmahugh/archive/2006/08/08/692600.aspx
 [2]: http://serverfault.com/questions/19060/why-are-docx-xlsx-pptx-downloading-from-webserver-as-zip-files
