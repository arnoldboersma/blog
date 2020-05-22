
---
title: "Ajax updatepanel SharePoint 2007"
date: 2010-03-22T08:37:00+08:00
lastmod: 2012-06-06T23:00:16+08:00
draft: false
categories: ["SharePoint", ]
tags: ["Ajax", "Sharepoint 2007", "UpdatePanel", ]
---


At a customer place we had issue’s after deploying a page with an Ajax updatepanel, on the development en test environment everything worked fine but on the customer’s acceptance and production environment the first postback in the updatepanel worked but after that the updatepanel didn’t react anymore.

After my collegue and I found the blog post of Mike Ammerlaan we could solve the problem pretty fast.

from [Mike Ammerlaan’s Blog](http://sharepoint.microsoft.com/blogs/mike/Lists/Posts/Post.aspx?ID=3):

> **Using UpdatePanels within SharePoint**
> 
> UpdatePanels are a very useful addition to ASP.NET AJAX, and represent the simplest way to convert existing, standard ASP.NET controls and parts to take advantage of Ajax techniques.  However, there are some changes within Windows SharePoint Services which may get in the way of working with ASP.NET AJAX.
> 
> Windows SharePoint Services JavaScript has a “form onSubmit wrapper” which is used to override the default form action.  This work is put in place to ensure that certain types of URLs, which may contain double byte characters, will fully work across most postback and asynchronous callback scenarios.  However, if your scenarios do not involve double byte character URLs, you may successful disable this workaround and gain the ability to use ASP.NET AJAX UpdatePanels.

For the page’s were we needed the updatepanel we placed the following script:

<span style="color: blue;"><</span><span style="color: maroon;">script </span><span style="color: red;">type</span><span style="color: blue;">="text/javascript"> </span><span style="color: #006400;"><!-- // Every single time the page loads, including every single AJAX partial postback </span><span style="color: blue;">function </span>pageLoad() {
        _spOriginalFormAction = document.forms[0].action; _spSuppressFormOnSubmitWrapper = <span style="color: blue;">true </span>}
<span style="color: #006400;">--> </span><span style="color: blue;"></</span><span style="color: maroon;">script</span><span style="color: blue;">> </span>

Most important of this script is that the fix needs to be applied after every pageload, otherwise only the first postback works fine.

