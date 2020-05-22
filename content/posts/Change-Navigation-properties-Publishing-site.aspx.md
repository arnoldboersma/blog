
---
title: "Change Navigation properties Publishing site"
date: 2011-08-14T05:15:00+08:00
lastmod: 2012-06-06T22:52:52+08:00
draft: false
categories: []
tags: []
---


[http://blogs.msdn.com/b/vesku/archive/2007/03/23/controlling-navigation-options-from-the-onet-xml.aspx](http://blogs.msdn.com/b/vesku/archive/2007/03/23/controlling-navigation-options-from-the-onet-xml.aspx "http://blogs.msdn.com/b/vesku/archive/2007/03/23/controlling-navigation-options-from-the-onet-xml.aspx")

Planning navigation in a SharePoint solution is essential

In a publishing site you have the following options to change the navigaion setting.

[![SNAGHTML134d78f](http://www.arnoldboersma.nl/image.axd?picture=SNAGHTML134d78f_thumb.png "SNAGHTML134d78f")](http://www.arnoldboersma.nl/image.axd?picture=SNAGHTML134d78f.png)

When rolling out a new project where you create the whole site with one script it is a lot of work to change the navigation settings of a site.

There are possibilities to change the navigation options in a couple of ways.

Feature: Portal Navigation Properties {541F5F57-C847-4e16-B59A-B31E90E6F9EA}

Example!!

        <span style="color: blue;"><</span><span style="color: #a31515;">Feature </span><span style="color: red;">ID</span><span style="color: blue;">=</span>"<span style="color: blue;">541F5F57-C847-4e16-B59A-B31E90E6F9EA</span>"<span style="color: blue;">> <!-- </span><span style="color: green;">Per-Web Portal Navigation Properties</span><span style="color: blue;">--> <</span><span style="color: #a31515;">Properties </span><span style="color: red;">xmlns</span><span style="color: blue;">=</span>"<span style="color: blue;">http://schemas.microsoft.com/sharepoint/</span>"<span style="color: blue;">> <</span><span style="color: #a31515;">Property </span><span style="color: red;">Key</span><span style="color: blue;">=</span>"<span style="color: blue;">InheritGlobalNavigation</span>" <span style="color: red;">Value</span><span style="color: blue;">=</span>"<span style="color: blue;">true</span>"<span style="color: blue;">/> <</span><span style="color: #a31515;">Property </span><span style="color: red;">Key</span><span style="color: blue;">=</span>"<span style="color: blue;">GlobalIncludeSubSites</span>" <span style="color: red;">Value</span><span style="color: blue;">=</span>"<span style="color: blue;">true</span>"<span style="color: blue;">/> <</span><span style="color: #a31515;">Property </span><span style="color: red;">Key</span><span style="color: blue;">=</span>"<span style="color: blue;">CurrentIncludeSubSites</span>" <span style="color: red;">Value</span><span style="color: blue;">=</span>"<span style="color: blue;">true</span>"<span style="color: blue;">/> <</span><span style="color: #a31515;">Property </span><span style="color: red;">Key</span><span style="color: blue;">=</span>"<span style="color: blue;">IncludePages</span>" <span style="color: red;">Value</span><span style="color: blue;">=</span>"<span style="color: blue;">false</span>" <span style="color: blue;">/> </</span><span style="color: #a31515;">Properties</span><span style="color: blue;">> </</span><span style="color: #a31515;">Feature</span><span style="color: blue;">> </span>

TEMP

// Properties   
public AutomaticSortingMethod AutomaticSortingMethod { get; set; }   
public int CurrentDynamicChildLimit { get; set; }   
public bool CurrentIncludePages { get; set; }   
public bool CurrentIncludeSubSites { get; set; }   
private NodeTypes CurrentIncludeTypes { get; set; }   
internal Dictionary<Guid, bool> CurrentNavigationExcludes { get; }   
public SPNavigationNodeCollection CurrentNavigationNodes { get; }   
private bool? DeprecatedIncludePages { get; }   
private bool? DeprecatedIncludeSubSites { get; }   
public int GlobalDynamicChildLimit { get; set; }   
public bool GlobalIncludePages { get; set; }   
public bool GlobalIncludeSubSites { get; set; }   
private NodeTypes GlobalIncludeTypes { get; set; }   
internal Dictionary<Guid, bool> GlobalNavigationExcludes { get; }   
public SPNavigationNodeCollection GlobalNavigationNodes { get; }   
public bool InheritCurrent { get; set; }   
public bool InheritGlobal { get; set; }   
public OrderingMethod OrderingMethod { get; set; }   
public bool ShowSiblings { get; set; }   
public bool SortAscending { get; set; }

TEMP

All options:

case "InheritGlobalNavigation":   
    publishingWeb.Navigation.InheritGlobal = bool.Parse(property.Value);   
    break;

case "InheritCurrentNavigation":   
    publishingWeb.Navigation.InheritCurrent = bool.Parse(property.Value);   
    break;

case "ShowSiblings":   
    publishingWeb.Navigation.ShowSiblings = bool.Parse(property.Value);   
    break;

case "IncludeSubSites":   
{   
    bool flag = bool.Parse(property.Value);   
    publishingWeb.Navigation.GlobalIncludeSubSites = flag;   
    publishingWeb.Navigation.CurrentIncludeSubSites = flag;   
    break;   
}   
case "IncludePages":   
{   
    bool flag2 = bool.Parse(property.Value);   
    publishingWeb.Navigation.GlobalIncludePages = flag2;   
    publishingWeb.Navigation.CurrentIncludePages = flag2;   
    break;   
}   
case "GlobalIncludeSubSites":   
    publishingWeb.Navigation.GlobalIncludeSubSites = bool.Parse(property.Value);   
    break;

case "GlobalIncludePages":   
    publishingWeb.Navigation.GlobalIncludePages = bool.Parse(property.Value);   
    break;

case "CurrentIncludeSubSites":   
    publishingWeb.Navigation.CurrentIncludeSubSites = bool.Parse(property.Value);   
    break;

case "CurrentIncludePages":   
    publishingWeb.Navigation.CurrentIncludePages = bool.Parse(property.Value);   
    break;

case "GlobalDynamicChildLimit":   
    publishingWeb.Navigation.GlobalDynamicChildLimit = int.Parse(property.Value);   
    break;

case "CurrentDynamicChildLimit":   
    publishingWeb.Navigation.CurrentDynamicChildLimit = int.Parse(property.Value);   
    break;

case "OrderingMethod":   
    publishingWeb.Navigation.OrderingMethod = (OrderingMethod) Enum.Parse(typeof(OrderingMethod), property.Value);   
    break;

case "AutomaticSortingMathod":   
case "AutomaticSortingMethod":   
    publishingWeb.Navigation.AutomaticSortingMethod = (AutomaticSortingMethod) Enum.Parse(typeof(AutomaticSortingMethod), property.Value);   
    break;

case "SortAscending":   
    publishingWeb.Navigation.SortAscending = bool.Parse(property.Value);   
    break;

case "IncludeInGlobalNavigation":   
    publishingWeb.IncludeInGlobalNavigation = bool.Parse(property.Value);   
    break;

case "IncludeInCurrentNavigation":   
    publishingWeb.IncludeInCurrentNavigation = bool.Parse(property.Value);   
    break;

