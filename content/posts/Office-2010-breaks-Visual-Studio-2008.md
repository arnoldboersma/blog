
---
title: "Office 2010 breaks Visual Studio 2008"
date: 2009-11-24T12:40:00+08:00
lastmod: 2012-06-10T13:12:43+08:00
draft: false
categories: ["Tools", ]
tags: ["Office 2010", "Visual studio", ]
---


After installing Office 2010 Beta I encountred issues with visual studio, the designer and split view freezes visual studio and caused the application to crash.   
on this [site](http://geekswithblogs.net/hinshelm/archive/2009/07/19/office-2010-gotcha-2-visual-studio-2008-locks.aspx) I found the solution.

If you get this problem then there is a simple solution, well, one that worked for me. You need to run a repair on the “Microsoft Visual Studio Web Authoring Component” that is part of Office 2007.

![Screenshot](https://public.bay.livefilestore.com/y1p74pFuLYAtX-sPMZCbiuVbmPvMBo4CeC9MFqeio2OBggYB_ODhwePIK1ft3uHaAQ6EHzTGp1cuId0oPLdTYt-gA/image_2.png?psid=1)

You can find the setup in the following locations:

**Windows 64bit**

C:\Program Files (x86)\Common Files\microsoft shared\OFFICE12\Office Setup Controller\Setup.exe

**Windows 32bit**

C:\Program Files\Common Files\microsoft shared\OFFICE12\Office Setup Controller\Setup.exe

