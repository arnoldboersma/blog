
---
title: "SharePoint 2010 Global List Id’s"
date: 2011-01-29T23:55:00+08:00
lastmod: 2012-06-10T13:13:51+08:00
draft: false
categories: ["SharePoint", ]
tags: ["SharePoint 2010", ]
---


[![defaultlists](https://public.bay.livefilestore.com/y1pK0CGP62PmZ3tkfiIAlp8H8YxMyPx2FkipJR8IJhLgvKB9tG3qwA2wQlmy547D35CX9YmKzGxyNPYc_3NzPvIrQ/defaultlists_thumb.png?psid=1 "defaultlists")](https://public.bay.livefilestore.com/y1poLLrRhyU_cPBKTI6QZi6unjMipeX8S16cjZTNVwDY_PPbaNAnmiY5J615fi2GXfx1RVWBwLLrimvAs-N6bkOjQ/defaultlists.png?psid=1)

Recently I needed to know the List id of the Theme’s list and wasn’t able to find a list on MSDN so I looked @ the SharePoint root to find out.

The SharePoint 2010 Global lists can be found @ “{SharePointRoot}\TEMPLATE\GLOBAL\Lists”, the following list definition’s are available.

All these lists are provisioned from the Global ONET.xml (“{SharePointRoot}\TEMPLATE\GLOBAL\XML\ONET.xml”) while creating a new Site collection.


<table style="width: 645px;" border="1" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="106">**Internal Name**</td>
<td valign="top" width="151">**Name**</td>
<td valign="top" width="319">**Description**</td>
<td valign="top" width="67">**Id**</td>
</tr>
<tr>
<td valign="top" width="107">


solutions

</td>
<td valign="top" width="151">Solution Gallery</td>
<td valign="top" width="318">Place to store Solutions</td>
<td valign="top" width="68">121</td>
</tr>
<tr>
<td valign="top" width="108">


mplib   

</td>
<td valign="top" width="150">Master Page Gallery</td>
<td valign="top" width="316">Place to store Master Pages</td>
<td valign="top" width="69">116</td>
</tr>
<tr>
<td valign="top" width="108">


users   

</td>
<td valign="top" width="150">User Information List</td>
<td valign="top" width="316">All users within the site collection</td>
<td valign="top" width="69">112</td>
</tr>
<tr>
<td valign="top" width="108">


webtemp 

</td>
<td valign="top" width="150">Site Template Gallery</td>
<td valign="top" width="316">Gallery for storing custom site designs</td>
<td valign="top" width="69">111</td>
</tr>
<tr>
<td valign="top" width="108">


wplib   

</td>
<td valign="top" width="150">Web Part Gallery</td>
<td valign="top" width="316">Gallery for storing web part definitions</td>
<td valign="top" width="69">113</td>
</tr>
<tr>
<td valign="top" width="108">


listtemp    


</td>
<td valign="top" width="150">List Template Gallery</td>
<td valign="top" width="316">Make a template available for use in list creation by adding to this gallery. Default list templates are not shown</td>
<td valign="top" width="69">114</td>
</tr>
<tr>
<td valign="top" width="108">


themes

</td>
<td valign="top" width="150">Theme Gallery</td>
<td valign="top" width="316">Use the theme gallery to store themes. The themes in this gallery can be used by this site or any of its subsites.</td>
<td valign="top" width="70">123</td>
</tr>
</tbody>
</table>

