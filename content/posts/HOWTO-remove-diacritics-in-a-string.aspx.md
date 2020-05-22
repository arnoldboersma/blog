
---
title: "HOWTO remove diacritics in a string"
date: 2009-10-20T13:31:00+08:00
lastmod: 2011-02-22T06:31:10+08:00
draft: false
categories: ["General", ]
tags: ["C#", ]
---


When comparing strings you sometimes need to ignore diacritics (accents) like é, ö etc.   
The following method removes the diacritics:

With the following extension method you can compare two strings for equality:  

<span style="color: blue">static string </span>RemoveDiacritics(<span style="color: blue">string </span>stIn)
{
    <span style="color: blue">string </span>stFormD = stIn.Normalize(<span style="color: #2b91af">NormalizationForm</span>.FormD);
    <span style="color: #2b91af">StringBuilder </span>sb = <span style="color: blue">new </span><span style="color: #2b91af">StringBuilder</span>();
    <span style="color: blue">for </span>(<span style="color: blue">int </span>ich = 0; ich < stFormD.Length; ich++)
    {
        <span style="color: #2b91af">UnicodeCategory </span>uc = <span style="color: #2b91af">CharUnicodeInfo</span>.GetUnicodeCategory(stFormD[ich]);
        <span style="color: blue">if </span>(uc != <span style="color: #2b91af">UnicodeCategory</span>.NonSpacingMark)
        {

            sb.Append(stFormD[ich]);
        }
    }
    <span style="color: blue">return </span>(sb.ToString().Normalize(<span style="color: #2b91af">NormalizationForm</span>.FormC));
}

<span style="color: blue">public static bool </span>Equals2(<span style="color: blue">this string </span>source, <span style="color: blue">string </span>toCheck)
{
    <span style="color: blue">return </span>RemoveDiacritics(source).ToLower().Trim() == 
        RemoveDiacritics(toCheck).ToLower().Trim();
}

The following code gives as result 'True'. 

<span style="color: blue">string </span>x = <span style="color: #a31515">"Azië"</span>;
<span style="color: blue">bool </span>equal = x.Equals2(<span style="color: #a31515">"azie"</span>);

[ ](http://11011.net/software/vspaste)

