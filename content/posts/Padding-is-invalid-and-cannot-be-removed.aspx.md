
---
title: "Padding is invalid and cannot be removed"
date: 2009-10-10T13:14:00+08:00
lastmod: 2011-02-22T06:31:10+08:00
draft: false
categories: ["General", ]
tags: ["C#", "IIS", "WebResource.axd", ]
---


After deploying my asp.net site to a webhosting company I suddenly was faced with error's when requesting WebResource.axd.

The following error occured:   
Padding is invalid and cannot be removed.
  <table border="0" cellspacing="0" cellpadding="2" width="668"><tbody>     <tr>       <td valign="top" width="666">         

Stacktrace:    at System.Security.Cryptography.RijndaelManagedTransform.DecryptData(Byte[] inputBuffer, Int32 inputOffset, Int32 inputCount, Byte[]& outputBuffer, Int32 outputOffset, PaddingMode paddingMode, Boolean fLast)   
   at System.Security.Cryptography.RijndaelManagedTransform.TransformFinalBlock(Byte[] inputBuffer, Int32 inputOffset, Int32 inputCount)   
   at System.Security.Cryptography.CryptoStream.FlushFinalBlock()   
   at System.Web.Configuration.MachineKeySection.EncryptOrDecryptData(Boolean fEncrypt, Byte[] buf, Byte[] modifier, Int32 start, Int32 length, IVType ivType, Boolean useValidationSymAlgo)   
   at System.Web.UI.Page.DecryptStringWithIV(String s, IVType ivType)   
   at System.Web.UI.Page.DecryptString(String s)   
   at System.Web.Handlers.ScriptResourceHandler.DecryptParameter(NameValueCollection queryString)
       </td>     </tr>   </tbody></table>  

The solution for this problem for me was to set a fixed machine key in the web.config file.   
For this you can use a [machineKey generator](http://www.orcsweb.com/articles/aspnetmachinekey.aspx) or generate your own if you know how to do it (random chars will not work). 

  <span style="color: blue"><</span><span style="color: #a31515">system.web</span><span style="color: blue">>
    <</span><span style="color: #a31515">machineKey    </span><span style="color: red">validationKey</span><span style="color: blue">=</span>'<span style="color: blue">A06BDCF2F6CF.A.VERY.LONG.44F13E76184945A7C477601</span>'
        <span style="color: red">decryptionKey</span><span style="color: blue">=</span>'<span style="color: blue">99079B21C2F3644.A.BIT.SHORTER.BB81C7E9D58378</span>'
        <span style="color: red">validation</span><span style="color: blue">=</span>'<span style="color: blue">SHA1</span>'<span style="color: blue">/>
  </</span><span style="color: #a31515">system.web</span><span style="color: blue">>
</span>

[](http://11011.net/software/vspaste)



For more detailed information you can read the blog post from [José Antonio](http://jagbarcelo.blogspot.com/2009/08/solution-padding-invalid-cannot-be.html)

