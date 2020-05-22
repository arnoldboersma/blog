
---
title: "Enable anonymous access to SharePoint 2013 – Part 1- Configure Web Application and SiteCollection"
date: 2014-04-03T12:59:27+08:00
lastmod: 2014-04-11T00:57:15+08:00
draft: false
categories: ["SharePoint", ]
tags: ["SharePoint 2013", "Anonymous Access", ]
---


This blog post describes how you can enable anonymous access to your SharePoint environment, anonymous access is often used when creating public facing websites.

<font color="#ff0000">Update 04/11/2014 Added FAQ item open office documents for anonymous users.</font>

Part 1: Configure Web Application and Site Collection – This Article

Part 2: Configure for CSOM and REST Api

## Preparing the Web Application

On Web Application level your IIS sites needs to know that it needs to accept anonymous requests, enabling anonymous access in SharePoint will enable the **Anonymous Authentication** provider for the Web Application across the Farm.
 ![](https://r1gs0g.dm2304.livefilestore.com/y2pLZQMdObhblixOHGZGznhBqdYtNvWkqR9jgi6Tz_t0ofg-fbdUptF7k3jBaJHA__GUCJfSg25QM3hwLfM0RBocb_EyJ447s95WYI-6JlcQyE/1iis.png?psid=1)   

### Configure anonymous access

#### Central Admin

*   Login to Central Administration 
*   Open Manage Web applications 
*   Select the Web Application where you want to enable Anonymous Access for 
*   Click on **Authentication Providers** in the ribbon and select the Zone 
*   Select **Enable Anonymous Access**, then click **Save** to update all IIS instances in the Farm ![](https://rvgs0g.dm2301.livefilestore.com/y2prxvC26Lixy8PnuJ98hjS09N90VdJ9Tj_ksOq_QtwzIrwldJary74bfi5GrlPVi12LG8nPY4HdSz519AxwAS4XqV82C_mAf3dGlDdBcNx4rE/2ca.png?psid=1)   

####  

#### PowerShell

<span class="rem">#Enable anonymous access on webapplication</span>
Write-Host -ForegroundColor White <span class="str">" - Set Anonymous access to webapplication"</span>
$WebApp = Get-SPWebApplication -Identity [http://root.contoso.com
<span class="rem">#In</span>](http://root.contoso.com#In) this sample anonymous access is set to the default zone
$Zone=[Microsoft.SharePoint.Administration.SPUrlZone]::Default 
$i = $WebApp.IisSettings[$Zone]
<span class="kwrd">if</span>($i.AllowAnonymous <span class="preproc">-ne</span> $true){
   $i.AllowAnonymous = $true
   $WebApp.Update()
   $WebApp.ProvisionGlobally()
}<span class="kwrd">else</span>{
   Write-Host -ForegroundColor White <span class="str">" - Anonymous access already set"</span>
}

<style type="text/css">













.csharpcode, .csharpcode pre
{
	font-size: small;
	color: black;
	font-family: consolas, "Courier New", courier, monospace;
	background-color: #ffffff;
	/*white-space: pre;*/
}
.csharpcode pre { margin: 0em; }
.csharpcode .rem { color: #008000; }
.csharpcode .kwrd { color: #0000ff; }
.csharpcode .str { color: #006080; }
.csharpcode .op { color: #0000c0; }
.csharpcode .preproc { color: #cc6633; }
.csharpcode .asp { background-color: #ffff00; }
.csharpcode .html { color: #800000; }
.csharpcode .attr { color: #ff0000; }
.csharpcode .alt 
{
	background-color: #f4f4f4;
	width: 100%;
	margin: 0em;
}
.csharpcode .lnum { color: #606060; }</style>



### Client callable settings

By default not all operation are allowed for anonymous users, some operations are blocked 

With PowerShell you can get a list of restricted types with the following PowerShell code:

$wa = Get-SPWebApplication -Identity <span class="str">"http://root.contoso.com"</span>
$wa.ClientCallableSettings.AnonymousRestrictedTypes

<style type="text/css">













.csharpcode, .csharpcode pre
{
	font-size: small;
	color: black;
	font-family: consolas, "Courier New", courier, monospace;
	background-color: #ffffff;
	/*white-space: pre;*/
}
.csharpcode pre { margin: 0em; }
.csharpcode .rem { color: #008000; }
.csharpcode .kwrd { color: #0000ff; }
.csharpcode .str { color: #006080; }
.csharpcode .op { color: #0000c0; }
.csharpcode .preproc { color: #cc6633; }
.csharpcode .asp { background-color: #ffff00; }
.csharpcode .html { color: #800000; }
.csharpcode .attr { color: #ff0000; }
.csharpcode .alt 
{
	background-color: #f4f4f4;
	width: 100%;
	margin: 0em;
}
.csharpcode .lnum { color: #606060; }</style>



![](https://rvgs0g.dm2301.livefilestore.com/y2pSsFf4XHMlr45t2XtKNZ5O8Ajy06yIVQ68fh64IQ36cUYS16YY5TooeDkh8xPCVb3Dx1WiDsUo35c_jpjD18HD2qi8KytEwBoi9-ds8vY7Sk/3ps1.png?psid=1)

Most interesting restricted type is the method **GetItems** on SPList, this means that it is not possible to retrieve List items with the client object model.

#### Enable restricted types

With PowerShell it is possible to remove the restrictions of the types that you want to allow anonymous users to use.

Write-Host <span class="str">"Remove Anonymous restricted type GetItems."</span>
$webApp = Get-SPWebApplication -Identity $webApplication
$webApp.ClientCallableSettings.AnonymousRestrictedTypes.Remove( [Microsoft.SharePoint.SPList],<span class="str">"GetItems"</span>)
$webApp.Update()

<style type="text/css">













.csharpcode, .csharpcode pre
{
	font-size: small;
	color: black;
	font-family: consolas, "Courier New", courier, monospace;
	background-color: #ffffff;
	/*white-space: pre;*/
}
.csharpcode pre { margin: 0em; }
.csharpcode .rem { color: #008000; }
.csharpcode .kwrd { color: #0000ff; }
.csharpcode .str { color: #006080; }
.csharpcode .op { color: #0000c0; }
.csharpcode .preproc { color: #cc6633; }
.csharpcode .asp { background-color: #ffff00; }
.csharpcode .html { color: #800000; }
.csharpcode .attr { color: #ff0000; }
.csharpcode .alt 
{
	background-color: #f4f4f4;
	width: 100%;
	margin: 0em;
}
.csharpcode .lnum { color: #606060; }</style>



## Configure site collection for anonymous access

After anonymous access is enabled on the Web Application the site collection needs to be configured for anonymous access, anonymous access can be set through the user interface or by PowerShell.

### Site Settings

*   Open **Site Settings** on the root web of the site collection 
*   Open **Site Permissions** 
*   Select **Anonymous Access** in the ribbon  ![](https://rvgs0g.dm1.livefilestore.com/y2pz6xJpID44vlOgG1NncaCQl6TM13iAFmQLLaNWB76s4-o3M0OF2xDkg7-Ja64HwchBcDUOLbAvQdds-kTnzBbjCORHLSIAHp_n4kYT1-FcMM/4ss1.png?psid=1) 

*   Select **Entire Website** and click **Ok 
        
![](https://rvgs0g.dm2302.livefilestore.com/y2p4U6kHpV3lyLZVRQaLdFuVVCaiXhUgo1k3XGu-FjQnHi_AtWR8MAL77Pcn2lifI0bz6cc1gSiE4ZDtmA0vlI0fV0ubNzEtWFg68OGIEy4JjU/5ss2.png?psid=1) **



### PowerShell

The same settings can be set with the following PowerShell Script

$siteCollectionUrl = <span class="str">"http://anonymous.contoso.com"</span>
$web = Get-SPWeb $siteCollectionUrl
$web.AnonymousState = [Microsoft.SharePoint.SPWeb+WebAnonymousState]::Enabled
$web.AnonymousPermMask64 = <span class="str">"Open, ViewPages, ViewListItems"</span>
$web.Update()

<style type="text/css">









.csharpcode, .csharpcode pre
{
	font-size: small;
	color: black;
	font-family: consolas, "Courier New", courier, monospace;
	background-color: #ffffff;
	/*white-space: pre;*/
}
.csharpcode pre { margin: 0em; }
.csharpcode .rem { color: #008000; }
.csharpcode .kwrd { color: #0000ff; }
.csharpcode .str { color: #006080; }
.csharpcode .op { color: #0000c0; }
.csharpcode .preproc { color: #cc6633; }
.csharpcode .asp { background-color: #ffff00; }
.csharpcode .html { color: #800000; }
.csharpcode .attr { color: #ff0000; }
.csharpcode .alt 
{
	background-color: #f4f4f4;
	width: 100%;
	margin: 0em;
}
.csharpcode .lnum { color: #606060; }</style>



> Be aware that anonymous settings has to be set on all sites and lists where you need anonymous users to have access, pay attention to broken security inheritance!

It is now possible to access the site collection anonymous, the following blog post will focus on accessing the site collection anonymous with the Javascript Object Model (JSOM) and REST Api Index.![](https://rvgs0g.dm2301.livefilestore.com/y2pcXKJKYN9qtwsAbU0T-o_1DtHNNqDID04Jzj0yI0iGsdZMjK6Flnw_0uLM9a11On1a5Aq58xzMebfhczaYaFckhx_l2iPLLPNiRSzms17s9w/6end.png?psid=1)

## FAQ

Q: How to open Office documents for anonymous users without getting an 401 Unauthorized exception

A: Add the “OpenItems” permission to the default AnonymousPermMask64, there is a [blog post](http://blogs.technet.com/b/acasilla/archive/2011/10/01/add-openitems-permission-to-the-default-anonymouspermmask64-so-that-you-can-open-office-docs-within-an-anonymous-accessible-sharepoint-site.aspx) from Anthony with a PowerShell script to update the permission mask.

