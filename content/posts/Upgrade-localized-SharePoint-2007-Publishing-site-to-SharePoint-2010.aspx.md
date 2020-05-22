
---
title: "Upgrade localized SharePoint 2007 Publishing site to SharePoint 2010"
date: 2011-02-22T06:09:00+08:00
lastmod: 2012-06-10T12:47:52+08:00
draft: false
categories: ["SharePoint", ]
tags: ["SharePoint 2010", "WCM", "Publishing", ]
---


[![publishing_error](https://public.bay.livefilestore.com/y1p8c0Pe5mWePsv60KNPafFPeGb2TpkQ6PEZoyCg2v04YnboqOfVknpoGza3jfGhruLLxV8U9egN_Le8zP7H-VRyw/publishing_error.png?psid=1 "publishing_error")](https://public.bay.livefilestore.com/y1p8c0Pe5mWePsv60KNPafFPeGb2TpkQ6PEZoyCg2v04YnboqOfVknpoGza3jfGhruLLxV8U9egN_Le8zP7H-VRyw/publishing_error.png?psid=1)

<span style="color: #ff0000;">Update: March 11 2011</span>

<span style="color: #ff0000;">Microsoft KB2484317 for issue with solution</span>

[http://support.microsoft.com/kb/2484317](http://support.microsoft.com/kb/2484317 "http://support.microsoft.com/kb/2484317")

After a succesfull (according to upgrade log file) upgrade of a SharePoint 2007 localized publishing site, the site is not working, the following error occurs when accessing the portal:

> *Ongeldig SPListItem. Het opgegeven SPListItem is niet compatibel met een publicatiepagina.*
> 
> *[ArgumentException: Ongeldig SPListItem. Het opgegeven SPListItem is niet compatibel met een publicatiepagina.]
> Microsoft.SharePoint.Publishing.PublishingPage.GetPublishingPage(SPListItem sourceListItem) +303
> Microsoft.SharePoint.Publishing.Internal.WebControls.PublishingPageStateControl.RaisePostBackEventForPageRouting(String eventArgument, SPRibbonCommandHandler control, RaisePostBackEventDelegate raisePostBackEventDelegate) +110
> Microsoft.SharePoint.Publishing.Internal.WebControls.PublishingPageSaveAndStopEditHandler.RaisePostBackEvent(String eventArgument) +134
> System.Web.UI.Page.RaisePostBackEvent(IPostBackEventHandler sourceControl, String eventArgument) +29
> System.Web.UI.Page.ProcessRequestMain(Boolean includeStagesBeforeAsyncPoint, Boolean includeStagesAfterAsyncPoint) +2981*

The same message in English:

> *"Invalid SPListItem. The SPListItem provided is not compatible with a Publishing Page."*
> 
>  

## Problem

Publishing sites in SharePoint 2007 has a default location for the publishing pages at ‘/Pages/’ in SharePoint 2010 the location has a new name in the Dutch Language pack of ‘/Paginas/’

The change has been made in the following resource file: osrvcore.nl-NL.resx

<Data Name="List_Pages_UrlName">   
  <Value><span style="background-color: #ffff00;">Paginas</span></Value>   
</Data>

## Quick Solution

This is not a real solution but this proves that the real solution solves the problem.

Step 1: change in the osrvcore.nl-NL.resx the following lines:

<Data Name="List_Pages_UrlName">   
<Value><span style="background-color: #ffff00;">Paginas</span></Value>   
</Data>

by:

<Data Name="List_Pages_UrlName">   
<Value><span style="background-color: #ffff00;">Pages</span></Value>   
</Data>

Warning: it is not supported to change the default resource files.

After saving the file and performing an IISreset the site is working with no problems, now we know this we can solve the issue in a more robust way by moving the ‘Pages’ to ‘Paginas’ location.

## Real Solution

After upgrading localized publishing sites the following steps needs to be performed:

Step 1: Move your publishing pages inside the pages library to ‘Paginas’

Create a console application an perform following code on the server, the same can be accomplished by writing a PowerShell script. In this example I know that I only need to change Dutch sites (1043).

<span style="color: blue;">using </span>(<span style="color: #2b91af;">SPSite </span>sitecollection = <span style="color: blue;">new </span><span style="color: #2b91af;">SPSite</span>(sitecollectionUrl))
{
    <span style="color: blue;">foreach </span>(<span style="color: #2b91af;">SPWeb </span>web <span style="color: blue;">in </span>sitecollection.AllWebs)
    {
        <span style="color: green;">//Dutch language </span><span style="color: blue;">if </span>(web.Language == 1043)
        {
            <span style="color: blue;">foreach </span>(<span style="color: #2b91af;">SPList </span>item <span style="color: blue;">in </span>web.Lists)
            {
                <span style="color: blue;">if </span>(item.RootFolder.Url == <span style="color: #a31515;">"Pages"</span>)
                {
                    item.RootFolder.MoveTo(item.RootFolder.Url.Replace(<span style="color: #a31515;">"Pages"</span>, <span style="color: #a31515;">"Paginas"</span>));
                    item.Update();
                }
            }
        }
    }
}

Step 2: Set the welcomepage location to the new default.aspx

Execute the following code on the server through a console app.

<span style="color: blue;">using </span>(<span style="color: #2b91af;">SPSite </span>sitecollection = <span style="color: blue;">new </span><span style="color: #2b91af;">SPSite</span>(sitecollectionUrl))
{
    <span style="color: blue;">foreach </span>(<span style="color: #2b91af;">SPWeb </span>web <span style="color: blue;">in </span>sitecollection.AllWebs)
    {
<span style="color: blue;">if </span>(web.Language == 1043)
        {
            <span style="color: blue;">if </span>(<span style="color: #2b91af;">PublishingWeb</span>.IsPublishingWeb(web))
            {
                <span style="color: #2b91af;">PublishingWeb </span>publishingWeb = <span style="color: #2b91af;">PublishingWeb</span>.GetPublishingWeb(web);
                <span style="color: #2b91af;">SPFile </span>welcomePage = <span style="color: blue;">null</span>;
                welcomePage = web.GetFile(<span style="color: #a31515;">"Paginas/default.aspx"</span>);

                <span style="color: blue;">if </span>(welcomePage != <span style="color: blue;">null</span>)
                {
<span style="color: gray;">//Set default welcomepage</span>

                    publishingWeb.DefaultPage = welcomePage;
                    publishingWeb.Update();
                }
            }        
        }
    }
}

Step 3: reactivate publishing feature for each site.

In this situation we only had a couple of subsites so we manually disabled and activated the feature for each sub site.

[![publishing_feature](https://public.bay.livefilestore.com/y1pcTgUmJufH3W1R52CgteHEbcokR7Gsd3jzKqAZFAtQWsf-rS-ClowG-cY49mpNXyc_jrUFFJwBDZFOtRmVTcpYA/publishing_feature.png?psid=1 "publishing_feature")](https://public.bay.livefilestore.com/y1pcTgUmJufH3W1R52CgteHEbcokR7Gsd3jzKqAZFAtQWsf-rS-ClowG-cY49mpNXyc_jrUFFJwBDZFOtRmVTcpYA/publishing_feature.png?psid=1)

