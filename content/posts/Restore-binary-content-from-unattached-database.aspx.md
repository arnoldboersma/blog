
---
title: "Restore binary content from unattached database"
date: 2011-09-12T07:44:00+08:00
lastmod: 2012-06-09T14:09:16+08:00
draft: false
categories: ["SharePoint", ]
tags: ["SharePoint 2010; Unattached Database; PowerShell", ]
---


## Introduction

When files are accidentally removed from a SharePoint 2010 content database in most circumstances they can be restored through the recycle bin, when files are also removed from the recycle bin it is not possible to restore files on item level basis.

When you want to restore those file you have the possibility to connect to a database that is not attached to a SharePoint farm.

## Attach backup of database to SQL server

The following MSDN article describes how to attach a database backup file to SQL server: [http://msdn.microsoft.com/en-us/library/ms186390.aspx](http://msdn.microsoft.com/en-us/library/ms186390.aspx "http://msdn.microsoft.com/en-us/library/ms186390.aspx").

Make sure the the account that is used to run the PowerShell operations has read access on the SQL server database

## Connect to unattached database

Start a SharePoint 2010 PowerShell console.

Connect the database

*$db = Get-SPContentDatabase -DatabaseName "NameOffDataBase" -ConnectAsUnattachedDatabase -DatabaseServer "databaseserver"*

At this moment the SharePoint API can be used to access content in the content database, execute following command:

*$db.Sites*

![](/IMAGES%2f2012%2f06%2fimage_2.png.jpgx)

## Example

We want to recover all User Profile images in the MySite Host site collection

*Add-PSSnapin Microsoft.SharePoint.Powershell -ErrorAction SilentlyContinue #Create connection to the database $db = Get-SPContentDatabase -DatabaseName <span class="str">"UnattachedDB"</span> -ConnectAsUnattachedDatabase -DatabaseServer <span class="str">"localhost"</span> #Get the list you want to restore information from, <span class="kwrd">in</span> <span class="kwrd">this</span> example we use the first sitecollection <span class="kwrd">in</span> the content database $list = $db.Sites[0].rootweb.lists[<span class="str">"User Photos"</span>] #set the view to set the query to retrieve the list items $oView = $list.Views[<span class="str">"All Items"</span>] #set the folder you want*

<div id="scid:0767317B-992E-4b12-91E0-4F059A8CECA8:1956b219-b3f9-451c-ad59-91dbf4b892a7" class="wlWriterSmartContent" style="float: none; margin: 0px; display: inline; padding: 0px;">Tags van Technorati:</div>


* to retrieve $oFolderChild = $list.RootFolder.SubFolders[<span class="str">"Profile Pictures"</span>] #Get the list items $collListItems = $list.GetItemsInFolder($oView, $oFolderChild) Write-Host $collListItems.Count #Iterate over all list items and save the file to your local disk, make sure the file location exists! ForEach ($i <span class="kwrd">in</span> $collListItems ) { Write-Host $i.File.Item.Name $raw = $i.File.OpenBinary() $name = "d:\t\" + $i.File.Item.Name Add-Content $name -Value $raw -Encoding <span class="kwrd">byte</span> }*

