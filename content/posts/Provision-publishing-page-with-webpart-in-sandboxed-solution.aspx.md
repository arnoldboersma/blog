
---
title: "Provision publishing page with webpart in sandboxed solution"
date: 2010-08-17T19:59:00+08:00
lastmod: 2011-02-22T06:29:51+08:00
draft: false
categories: ["SharePoint", ]
tags: ["SharePoint 2010", "Sandbox", ]
---


While working with Sandboxed solutions in SharePoint 2010 I faced a problem with attaching a webpart to a publishing page.

The code I used for deploying a page:


<div style="border-bottom: silver 1px solid; text-align: left; border-left: silver 1px solid; padding-bottom: 4px; line-height: 12pt; background-color: #f4f4f4; margin: 20px 0px 10px; padding-left: 4px; width: 97.5%; padding-right: 4px; font-family: 'Courier New', courier, monospace; direction: ltr; max-height: 200px; font-size: 8pt; overflow: auto; border-top: silver 1px solid; cursor: text; border-right: silver 1px solid; padding-top: 4px" id="codeSnippetWrapper">
  <div style="border-bottom-style: none; text-align: left; padding-bottom: 0px; line-height: 12pt; border-right-style: none; background-color: #f4f4f4; padding-left: 0px; width: 100%; padding-right: 0px; font-family: 'Courier New', courier, monospace; direction: ltr; border-top-style: none; color: black; font-size: 8pt; border-left-style: none; overflow: visible; padding-top: 0px" id="codeSnippet">
    

<span style="color: #606060" id="lnum1">   1:</span> <span style="color: #0000ff"><</span><span style="color: #800000">Elements</span> <span style="color: #ff0000">xmlns</span><span style="color: #0000ff">="http://schemas.microsoft.com/sharepoint/"</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum2">   2:</span>   <span style="color: #0000ff"><</span><span style="color: #800000">Module</span> <span style="color: #ff0000">Name</span><span style="color: #0000ff">="PageInstance"</span>

<span style="color: #606060" id="lnum3">   3:</span>         <span style="color: #ff0000">Url</span><span style="color: #0000ff">="Pages"</span>

<span style="color: #606060" id="lnum4">   4:</span>         <span style="color: #ff0000">Path</span><span style="color: #0000ff">="PageInstance"</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum5">   5:</span>  

<span style="color: #606060" id="lnum6">   6:</span>     <span style="color: #0000ff"><</span><span style="color: #800000">File</span> <span style="color: #ff0000">Url</span><span style="color: #0000ff">="test.aspx"</span> <span style="color: #ff0000">Path</span><span style="color: #0000ff">="default.aspx"</span> <span style="color: #ff0000">Type</span><span style="color: #0000ff">="GhostableInLibrary"</span> <span style="color: #ff0000">IgnoreIfAlreadyExists</span><span style="color: #0000ff">="TRUE"</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum7">   7:</span>       <span style="color: #0000ff"><</span><span style="color: #800000">Property</span> <span style="color: #ff0000">Name</span><span style="color: #0000ff">="Title"</span> <span style="color: #ff0000">Value</span><span style="color: #0000ff">="Test Title"</span> <span style="color: #0000ff">/></span>

<span style="color: #606060" id="lnum8">   8:</span>       <span style="color: #0000ff"><</span><span style="color: #800000">Property</span> <span style="color: #ff0000">Name</span><span style="color: #0000ff">="PublishingPageLayout"</span> <span style="color: #ff0000">Value</span><span style="color: #0000ff">="~SiteCollection/_catalogs/masterpage/customLayout.aspx, My Custom Layout"</span> <span style="color: #0000ff">/></span>

<span style="color: #606060" id="lnum9">   9:</span>       <span style="color: #0000ff"><</span><span style="color: #800000">Property</span> <span style="color: #ff0000">Name</span><span style="color: #0000ff">="ContentType"</span> <span style="color: #ff0000">Value</span><span style="color: #0000ff">="$Resources:cmscore,contenttype_pagelayout_name;"</span> <span style="color: #0000ff">/></span>

<span style="color: #606060" id="lnum10">  10:</span>       <span style="color: #0000ff"><</span><span style="color: #800000">Property</span> <span style="color: #ff0000">Name</span><span style="color: #0000ff">="PublishingPreviewImage"</span> <span style="color: #ff0000">Value</span><span style="color: #0000ff">="~SiteCollection/_catalogs/masterpage/$Resources:core,Culture;/Preview Images/ArticleLinks.png, ~SiteCollection/_catalogs/masterpage/$Resources:core,Culture;/Preview Images/ArticleLinks.png"</span> <span style="color: #0000ff">/></span>

<span style="color: #606060" id="lnum11">  11:</span>       <span style="color: #0000ff"><</span><span style="color: #800000">Property</span> <span style="color: #ff0000">Name</span><span style="color: #0000ff">="PublishingAssociatedContentType"</span> <span style="color: #ff0000">Value</span><span style="color: #0000ff">=";#CustomLayout - CustomPages;#0x010100C568DB52D9D0A14D9B2FDCC96666E9F2007948130EC3DB064584E219954237AF3900242457EFB8B24247815D688C526CD44D00AC43246F7BB54A1389CAF0BEC1C1AEE4;#"</span><span style="color: #0000ff">/></span>

<span style="color: #606060" id="lnum12">  12:</span>       <span style="color: #0000ff"><</span><span style="color: #800000">AllUsersWebPart</span> <span style="color: #ff0000">WebPartOrder</span><span style="color: #0000ff">="1"</span> <span style="color: #ff0000">WebPartZoneID</span><span style="color: #0000ff">="Main"</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum13">  13:</span>       <span style="color: #0000ff"><!</span>[CDATA[

<span style="color: #606060" id="lnum14">  14:</span>       <span style="color: #0000ff"><</span><span style="color: #800000">webParts</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum15">  15:</span>         <span style="color: #0000ff"><</span><span style="color: #800000">webPart</span> <span style="color: #ff0000">xmlns</span><span style="color: #0000ff">="http://schemas.microsoft.com/WebPart/v3"</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum16">  16:</span>           <span style="color: #0000ff"><</span><span style="color: #800000">metaData</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum17">  17:</span>             <span style="color: #0000ff"><</span><span style="color: #800000">type</span> <span style="color: #ff0000">name</span><span style="color: #0000ff">="CustomLayout.SandboxedVisualWebPart1.SandboxedVisualWebPart1, CustomLayout, Version=1.0.0.0, Culture=neutral, PublicKeyToken=9242a569ebe2d353"</span> <span style="color: #0000ff">/></span>

<span style="color: #606060" id="lnum18">  18:</span>             <span style="color: #0000ff"><</span><span style="color: #800000">importErrorMessage</span><span style="color: #0000ff">></span>$Resources:core,ImportErrorMessage;<span style="color: #0000ff"></</span><span style="color: #800000">importErrorMessage</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum19">  19:</span>             <span style="color: #0000ff"><</span><span style="color: #800000">Solution</span> <span style="color: #ff0000">SolutionId</span><span style="color: #0000ff">="245ba111-9620-424c-9a8f-9eceaa2d33c6"</span> <span style="color: #ff0000">xmlns</span><span style="color: #0000ff">="http://schemas.microsoft.com/sharepoint/"</span> <span style="color: #0000ff">/></span>

<span style="color: #606060" id="lnum20">  20:</span>           <span style="color: #0000ff"></</span><span style="color: #800000">metaData</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum21">  21:</span>           <span style="color: #0000ff"><</span><span style="color: #800000">data</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum22">  22:</span>             <span style="color: #0000ff"><</span><span style="color: #800000">properties</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum23">  23:</span>               <span style="color: #0000ff"><</span><span style="color: #800000">property</span> <span style="color: #ff0000">name</span><span style="color: #0000ff">="Title"</span> <span style="color: #ff0000">type</span><span style="color: #0000ff">="string"</span><span style="color: #0000ff">></span>SandboxedVisualWebPart1<span style="color: #0000ff"></</span><span style="color: #800000">property</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum24">  24:</span>               <span style="color: #0000ff"><</span><span style="color: #800000">property</span> <span style="color: #ff0000">name</span><span style="color: #0000ff">="Description"</span> <span style="color: #ff0000">type</span><span style="color: #0000ff">="string"</span><span style="color: #0000ff">></span>My custom sandbox web part<span style="color: #0000ff"></</span><span style="color: #800000">property</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum25">  25:</span>             <span style="color: #0000ff"></</span><span style="color: #800000">properties</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum26">  26:</span>           <span style="color: #0000ff"></</span><span style="color: #800000">data</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum27">  27:</span>         <span style="color: #0000ff"></</span><span style="color: #800000">webPart</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum28">  28:</span>       <span style="color: #0000ff"></</span><span style="color: #800000">webParts</span><span style="color: #0000ff">></span>     

<span style="color: #606060" id="lnum29">  29:</span>       ]]<span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum30">  30:</span>       <span style="color: #0000ff"></</span><span style="color: #800000">AllUsersWebPart</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum31">  31:</span>     <span style="color: #0000ff"></</span><span style="color: #800000">File</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum32">  32:</span>   <span style="color: #0000ff"></</span><span style="color: #800000">Module</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum33">  33:</span> <span style="color: #0000ff"></</span><span style="color: #800000">Elements</span><span style="color: #0000ff">></span>

</div>
</div>



 <span style="color: blue"><font style="background-color: #ffffff" color="#000000" face="Verdana"></font></span> <span style="color: blue"><font style="background-color: #ffffff" color="#000000" face="Verdana">The problems with this in a sandboxed solution are:</font></span>

<span style="color: blue"><font style="background-color: #ffffff" color="#000000" face="Verdana"></font></span>



<span style="color: blue"><font style="background-color: #ffffff" color="#000000" face="Verdana">1. Approval state is set to draft</font></span>

<span style="color: blue"><font style="background-color: #ffffff" color="#000000" face="Verdana">2. Webpart is not deployed</font></span>

<span style="color: blue"><font style="background-color: #ffffff" color="#000000" face="Verdana"></font></span>



<span style="color: blue"><font style="background-color: #ffffff" color="#000000" face="Verdana">I realized that there is a good reason that there are problems using this approach because SharePoint will try to load “default.aspx” which is stored in the “SharePoint Root\TEMPLATE\FEATURES\FEATURENAME folder, and Sandbox Solutions doesn’t have access to the SharePoint root.</font></span>

<span style="color: blue"><font style="background-color: #ffffff" color="#000000" face="Verdana"></font></span> 

<span style="color: blue"><font style="background-color: #ffffff" color="#000000" face="Verdana"></font></span> 

On MSDN I found that a module element accepts the “SetupPath” property ([http://msdn.microsoft.com/en-us/library/ms460356.aspx](http://msdn.microsoft.com/en-us/library/ms460356.aspx "http://msdn.microsoft.com/en-us/library/ms460356.aspx")) now I rewrote my feature with the “SetupPath” property.


<div style="border-bottom: silver 1px solid; text-align: left; border-left: silver 1px solid; padding-bottom: 4px; line-height: 12pt; background-color: #f4f4f4; margin: 20px 0px 10px; padding-left: 4px; width: 97.5%; padding-right: 4px; font-family: 'Courier New', courier, monospace; direction: ltr; max-height: 200px; font-size: 8pt; overflow: auto; border-top: silver 1px solid; cursor: text; border-right: silver 1px solid; padding-top: 4px" id="codeSnippetWrapper">
  <div style="border-bottom-style: none; text-align: left; padding-bottom: 0px; line-height: 12pt; border-right-style: none; background-color: #f4f4f4; padding-left: 0px; width: 100%; padding-right: 0px; font-family: 'Courier New', courier, monospace; direction: ltr; border-top-style: none; color: black; font-size: 8pt; border-left-style: none; overflow: visible; padding-top: 0px" id="codeSnippet">
    

<span style="color: #606060" id="lnum1">   1:</span> <span style="color: #0000ff"><</span><span style="color: #800000">Elements</span> <span style="color: #ff0000">xmlns</span><span style="color: #0000ff">="http://schemas.microsoft.com/sharepoint/"</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum2">   2:</span>   <span style="color: #0000ff"><</span><span style="color: #800000">Module</span> <span style="color: #ff0000">Name</span><span style="color: #0000ff">="PageInstance"</span>

<span style="color: #606060" id="lnum3">   3:</span>         <span style="color: #ff0000">Url</span><span style="color: #0000ff">="Pages"</span>

<span style="color: #606060" id="lnum4">   4:</span>         <span style="color: #ff0000">SetupPath</span><span style="color: #0000ff">="SiteTemplates\SPS"</span><span style="color: #0000ff">></span> <span style="color: #008000"><!-- Important for sandboxed solution!!! path in file element is using this path for finding location --></span>

<span style="color: #606060" id="lnum5">   5:</span>  

<span style="color: #606060" id="lnum6">   6:</span>     <span style="color: #0000ff"><</span><span style="color: #800000">File</span> <span style="color: #ff0000">Url</span><span style="color: #0000ff">="test.aspx"</span> <span style="color: #ff0000">Path</span><span style="color: #0000ff">="default.aspx"</span> <span style="color: #ff0000">Type</span><span style="color: #0000ff">="GhostableInLibrary"</span> <span style="color: #ff0000">IgnoreIfAlreadyExists</span><span style="color: #0000ff">="TRUE"</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum7">   7:</span>       <span style="color: #0000ff"><</span><span style="color: #800000">Property</span> <span style="color: #ff0000">Name</span><span style="color: #0000ff">="Title"</span> <span style="color: #ff0000">Value</span><span style="color: #0000ff">="Test Title"</span> <span style="color: #0000ff">/></span>

<span style="color: #606060" id="lnum8">   8:</span>       <span style="color: #0000ff"><</span><span style="color: #800000">Property</span> <span style="color: #ff0000">Name</span><span style="color: #0000ff">="PublishingPageLayout"</span> <span style="color: #ff0000">Value</span><span style="color: #0000ff">="~SiteCollection/_catalogs/masterpage/customLayout.aspx, My Custom Layout"</span> <span style="color: #0000ff">/></span>

<span style="color: #606060" id="lnum9">   9:</span>       <span style="color: #0000ff"><</span><span style="color: #800000">Property</span> <span style="color: #ff0000">Name</span><span style="color: #0000ff">="ContentType"</span> <span style="color: #ff0000">Value</span><span style="color: #0000ff">="$Resources:cmscore,contenttype_pagelayout_name;"</span> <span style="color: #0000ff">/></span>

<span style="color: #606060" id="lnum10">  10:</span>       <span style="color: #0000ff"><</span><span style="color: #800000">Property</span> <span style="color: #ff0000">Name</span><span style="color: #0000ff">="PublishingPreviewImage"</span> <span style="color: #ff0000">Value</span><span style="color: #0000ff">="~SiteCollection/_catalogs/masterpage/$Resources:core,Culture;/Preview Images/ArticleLinks.png, ~SiteCollection/_catalogs/masterpage/$Resources:core,Culture;/Preview Images/ArticleLinks.png"</span> <span style="color: #0000ff">/></span>

<span style="color: #606060" id="lnum11">  11:</span>       <span style="color: #0000ff"><</span><span style="color: #800000">Property</span> <span style="color: #ff0000">Name</span><span style="color: #0000ff">="PublishingAssociatedContentType"</span> <span style="color: #ff0000">Value</span><span style="color: #0000ff">=";#CustomLayout - CustomPages;#0x010100C568DB52D9D0A14D9B2FDCC96666E9F2007948130EC3DB064584E219954237AF3900242457EFB8B24247815D688C526CD44D00AC43246F7BB54A1389CAF0BEC1C1AEE4;#"</span><span style="color: #0000ff">/></span>

<span style="color: #606060" id="lnum12">  12:</span>       <span style="color: #0000ff"><</span><span style="color: #800000">AllUsersWebPart</span> <span style="color: #ff0000">WebPartOrder</span><span style="color: #0000ff">="1"</span> <span style="color: #ff0000">WebPartZoneID</span><span style="color: #0000ff">="Main"</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum13">  13:</span>       <span style="color: #0000ff"><!</span>[CDATA[

<span style="color: #606060" id="lnum14">  14:</span>       <span style="color: #0000ff"><</span><span style="color: #800000">webParts</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum15">  15:</span>         <span style="color: #0000ff"><</span><span style="color: #800000">webPart</span> <span style="color: #ff0000">xmlns</span><span style="color: #0000ff">="http://schemas.microsoft.com/WebPart/v3"</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum16">  16:</span>           <span style="color: #0000ff"><</span><span style="color: #800000">metaData</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum17">  17:</span>             <span style="color: #0000ff"><</span><span style="color: #800000">type</span> <span style="color: #ff0000">name</span><span style="color: #0000ff">="CustomLayout.SandboxedVisualWebPart1.SandboxedVisualWebPart1, CustomLayout, Version=1.0.0.0, Culture=neutral, PublicKeyToken=9242a569ebe2d353"</span> <span style="color: #0000ff">/></span>

<span style="color: #606060" id="lnum18">  18:</span>             <span style="color: #0000ff"><</span><span style="color: #800000">importErrorMessage</span><span style="color: #0000ff">></span>$Resources:core,ImportErrorMessage;<span style="color: #0000ff"></</span><span style="color: #800000">importErrorMessage</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum19">  19:</span>             <span style="color: #0000ff"><</span><span style="color: #800000">Solution</span> <span style="color: #ff0000">SolutionId</span><span style="color: #0000ff">="245ba111-9620-424c-9a8f-9eceaa2d33c6"</span> <span style="color: #ff0000">xmlns</span><span style="color: #0000ff">="http://schemas.microsoft.com/sharepoint/"</span> <span style="color: #0000ff">/></span>

<span style="color: #606060" id="lnum20">  20:</span>           <span style="color: #0000ff"></</span><span style="color: #800000">metaData</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum21">  21:</span>           <span style="color: #0000ff"><</span><span style="color: #800000">data</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum22">  22:</span>             <span style="color: #0000ff"><</span><span style="color: #800000">properties</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum23">  23:</span>               <span style="color: #0000ff"><</span><span style="color: #800000">property</span> <span style="color: #ff0000">name</span><span style="color: #0000ff">="Title"</span> <span style="color: #ff0000">type</span><span style="color: #0000ff">="string"</span><span style="color: #0000ff">></span>SandboxedVisualWebPart1<span style="color: #0000ff"></</span><span style="color: #800000">property</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum24">  24:</span>               <span style="color: #0000ff"><</span><span style="color: #800000">property</span> <span style="color: #ff0000">name</span><span style="color: #0000ff">="Description"</span> <span style="color: #ff0000">type</span><span style="color: #0000ff">="string"</span><span style="color: #0000ff">></span>My custom sandbox web part<span style="color: #0000ff"></</span><span style="color: #800000">property</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum25">  25:</span>             <span style="color: #0000ff"></</span><span style="color: #800000">properties</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum26">  26:</span>           <span style="color: #0000ff"></</span><span style="color: #800000">data</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum27">  27:</span>         <span style="color: #0000ff"></</span><span style="color: #800000">webPart</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum28">  28:</span>       <span style="color: #0000ff"></</span><span style="color: #800000">webParts</span><span style="color: #0000ff">></span>     

<span style="color: #606060" id="lnum29">  29:</span>       ]]<span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum30">  30:</span>       <span style="color: #0000ff"></</span><span style="color: #800000">AllUsersWebPart</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum31">  31:</span>     <span style="color: #0000ff"></</span><span style="color: #800000">File</span><span style="color: #0000ff">></span>

<span style="color: #606060" id="lnum32">  32:</span>   <span style="color: #0000ff"></</span><span style="color: #800000">Module</span><span style="color: #0000ff">></span>

</div>
</div>



At this point Path=”default.aspx” is pointing to the physic location: “SharePoint Root\TEMPLATE\SiteTemplates\SPS\default.apx”, so the “default.aspx” file in the module can be removed.

The file “default.aspx” as a template file for a publishing page.

After deploying the feature with this module the state of the page is Approved and the Webpart is available on the page.

