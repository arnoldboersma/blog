
---
title: "Using Fiddler with Visual Studio build in Webserver"
date: 2009-09-14T09:45:53+08:00
lastmod: 2011-02-22T06:31:01+08:00
draft: false
categories: ["Tools", ]
tags: ["Fiddler", "Tools", "Visual studio", ]
---


Today I needed to inspect the webtraffic to my WCF webservices, Fiddler was not able to see my webtraffic to localhost. To use fiddler with localhost these steps needs to be done: 1. Disable IPv6 in Fiddler configuration. [caption id="attachment_41" align="aligncenter" width="480" caption="Fiddler settings"]![Fiddler settings](http://arnoldboersma.files.wordpress.com/2009/09/fiddler_ipv6.png "fiddler_ipv6")[/caption] 2. Add <computername> to your hosts file and set it to 127.0.0.1. 127.0.0.1                       <computername> 3. request your webserver with [:http://<machinename>:<portnumber](http://<machinename>:<portnumber)></portnumber)></machinename>>. [caption id="attachment_43" align="aligncenter" width="480" caption="Fiddler Traffic"]![Fiddler Traffic](http://arnoldboersma.files.wordpress.com/2009/09/fiddler_traffic.png "Fiddler_traffic")[/caption]  

