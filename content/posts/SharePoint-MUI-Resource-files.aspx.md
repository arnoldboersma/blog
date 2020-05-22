
---
title: "SharePoint MUI Resource files"
date: 2011-05-11T06:16:00+08:00
lastmod: 2012-06-10T13:09:06+08:00
draft: false
categories: ["SharePoint", ]
tags: ["MUI", "SharePoint 2010", ]
---


In SharePoint 2010 you have the ability to configure alternate languages for a website, this feature enables that navigation and labels are presented in the user selected language without creating variations, when on a farm multiple language pack’s are deployed you can configure Alternate Languages on every site.

## Configure Multilangual User Interface

1. goto site settings –> Language Settings

[![image](https://public.bay.livefilestore.com/y1pzIZqiS1G4RMxFg94sXa8prLrEHes43DKVMDcCEvOwPPLnNsbsM5cjOq5WZ2V436c5U1wbTXPeiOBP7QiuXL18g/image_thumb.png?psid=1 "image")](https://public.bay.livefilestore.com/y1pzIZqiS1G4RMxFg94sXa8prLrEHes43DKVMDcCEvOwPPLnNsbsM5cjOq5WZ2V436c5U1wbTXPeiOBP7QiuXL18g/image_thumb.png?psid=1)

2. select the languages of choise.

3. press OK to save changes.

## Change Language on Site

The language of the current site can be changed by slecting the ‘Select Display Language’ in the user menu

[![image](https://public.bay.livefilestore.com/y1pEj4bw5Jsye3_vOWcItOZjF1qLxVaHSh_jo0TR-QIbt2nMgpLY69Ij5IPGgfwFbPN8v0wCScEG7coZeGTZqEkOA/image_thumb_1.png?psid=1 "image")](https://public.bay.livefilestore.com/y1pzIZqiS1G4RMxFg94sXa8prLrEHes43DKVMDcCEvOwPPLnNsbsM5cjOq5WZ2V436c5U1wbTXPeiOBP7QiuXL18g/image_thumb.png?psid=1)

## Using Labels in your solution

When you want to use different languages in your custom solutions it is possible to make use of resource files withing SharePoint.

The SharePoint object model provides Helper methods to get localized strings, the following article on MSDN explains how to use this: [http://msdn.microsoft.com/en-us/library/microsoft.sharepoint.utilities.sputility.getlocalizedstring.aspx](http://msdn.microsoft.com/en-us/library/microsoft.sharepoint.utilities.sputility.getlocalizedstring.aspx "http://msdn.microsoft.com/en-us/library/microsoft.sharepoint.utilities.sputility.getlocalizedstring.aspx")

### Get current language of user

With SPWeb.Language you get the ‘default’ language of the site in the case of alternate languages it is not sufficient to use this because a user could have set the language to a non ‘default’ language.

The current language of a user can be found at: <span style="color: #2b91af;">CultureInfo</span>.CurrentUICulture.LCID

The following Helper method can be used to retrieve localized strings with respect to the user selected language.

<span style="color: blue;">public static string </span>GetResourceString(<span style="color: blue;">string </span>resourceName, <span style="color: blue;">string </span>name)
{
    <span style="color: blue;">int </span>languageId = 1033;
    <span style="color: blue;">if </span>(<span style="color: #2b91af;">SPContext</span>.Current != <span style="color: blue;">null </span>&& <span style="color: #2b91af;">SPContext</span>.Current.Web != <span style="color: blue;">null</span>)
    {
        <span style="color: green;">//Set Language to default language of web object! </span>languageId = (<span style="color: blue;">int</span>)<span style="color: #2b91af;">SPContext</span>.Current.Web.Language; 
   
        <span style="color: green;">//if alternate language is selected by user! </span><span style="color: blue;">if </span>(<span style="color: #2b91af;">SPContext</span>.Current.Web.IsMultilingual &&
            <span style="color: #2b91af;">SPContext</span>.Current.Web.SupportedUICultures.Where(w => w.LCID == <span style="color: #2b91af;">CultureInfo</span>.CurrentUICulture.LCID).
                Any())
        {
            languageId = <span style="color: #2b91af;">CultureInfo</span>.CurrentUICulture.LCID;
        }
    }
    <span style="color: blue;">return </span><span style="color: #2b91af;">SPUtility</span>.GetLocalizedString(resourceName, name, (<span style="color: blue;">uint</span>)languageId);
}

