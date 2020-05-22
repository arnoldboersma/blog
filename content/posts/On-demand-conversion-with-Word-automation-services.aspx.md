
---
title: "On demand conversion with Word automation services"
date: 2013-03-05T13:39:00+08:00
lastmod: 2014-07-08T12:01:25+08:00
draft: false
categories: ["SharePoint", ]
tags: ["SharePoint 2013", "Word automation", "Development", ]
---


## Introduction

In SharePoint 2010 word automation services was introduced the service could be used to do a lot of batch conversion tasks. The implementation had a couple of drawback’s:

*   File generation has to wait for the timer job to execute 
*   Can only handle files in SharePoint 
*   No feedback when job is done in an easy way   

With SharePoint 2013 it is now possible to:

*   Convert documents on the fly 
*   Source file doesn’t need to exist in SharePoint 
*   Save converted files anywhere you want   

The following TechNet article describes the architecture of on demand file conversion: [What's new in Word Automation Services for developers](http://msdn.microsoft.com/en-us/library/jj163073.aspx#was15CreateOnDemandConversion "http://msdn.microsoft.com/en-us/library/jj163073.aspx#was15CreateOnDemandConversion")

## Code download

[Download solution](http://1drv.ms/1nbzK2i)

## Solution

With the following code examples I want to demonstrate how you can convert files on demand, in the example an word document can be converted on demand to a PDF document.

In the solution the following items are created:

*   Custom action with an Edit Control block 
*   Application page were the conversion takes place   

### Custom action

The custom action should be added to the EditControlBlock and only be available for word documents.

![](https://2kzfjq.bn1.livefilestore.com/y1p73gD1cA-plm6DiBZAFlRYrqCIInmUXB32wXI5FnyhERsmkC5h70tK3R5W5jQZztReqrqBr3k2cVeeqzxu2oRrLqhys3jG61z/converttopdfecb.png?psid=1)

Create custom action in Visual studio:

<?xml version=<span class="str">"1.0"</span> encoding=<span class="str">"utf-8"</span>?>
<Elements xmlns=<span class="str">"http://schemas.microsoft.com/sharepoint/"</span>>
  <CustomAction
       Id=<span class="str">"E0648444-7D5A-4917-875D-C594E4266282.CustomAction"</span>
       RegistrationType=<span class="str">"FileType"</span>
       RegistrationId=<span class="str">"docx"</span>
       Location=<span class="str">"EditControlBlock"</span>
       Sequence=<span class="str">"10001"</span>
       Title=<span class="str">"Convert to PDF"</span>
       ImageUrl=<span class="str">"/_layouts/15/images/icpdf.png"</span>>
    <UrlAction Url=<span class="str">"javascript:OpenPopUpPageWithTitle('{SiteUrl}/_layouts/Vivens.SP2013.WordServices/WordAutomationConverter.aspx?</span>

<span class="str">      ListId={ListId}&amp;ItemId={ItemId}&amp;ItemUrl={ItemUrl}', RefreshOnDialogClose, 600, 400,'Convert to PDF')"</span>/>
  </CustomAction>
</Elements>

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



### Application page

The application page is opened in a dialog window and from this page it is possible to initiate the on demand conversion.

![](https://16zfjq.bn1.livefilestore.com/y1p6VltAeHYMVEkJdW5MH8ynXILKsjdv9EoINdpePa2Kwe2h2KIK2GDQZZQr2SmnVSZWViwVYdEkwAQePFzBnFX9zIhOTDhL2Cb/converttopdf.png?psid=1)

WordAutomationConverter.aspx

<asp:Content ID=<span class="str">"Main"</span> ContentPlaceHolderID=<span class="str">"PlaceHolderMain"</span> runat=<span class="str">"server"</span>>
    <asp:Panel id=<span class="str">"pnlConversion"</span> runat=<span class="str">"server"</span>>
        <asp:Label ID=<span class="str">"lblInputItem"</span> runat=<span class="str">"server"</span> Text=<span class="str">"Label"</span>></asp:Label>

        <asp:Button ID=<span class="str">"btnConvertSharePointFile"</span> runat=<span class="str">"server"</span> Text=<span class="str">"Save document as PDF"</span> OnClick=<span class="str">"btnConvertSharePointFile_Click"</span> />
        <asp:Button ID=<span class="str">"btnDownloadAsPdf"</span> runat=<span class="str">"server"</span> Text=<span class="str">"Download file as PDF"</span> OnClick=<span class="str">"btnDownloadAsPdf_Click"</span> />
    </asp:Panel>
    <asp:Panel ID=<span class="str">"pnlCompleted"</span> runat=<span class="str">"server"</span> Visible=<span class="str">"false"</span>>
        <asp:Label ID=<span class="str">"lblConvertStatus"</span> runat=<span class="str">"server"</span> Text=<span class="str">"Label"</span>></asp:Label>
        <asp:Button ID=<span class="str">"btnReturnToList"</span> runat=<span class="str">"server"</span> Text=<span class="str">"Return to list"</span> OnClick=<span class="str">"btnReturnToList_Click"</span>  />
    </asp:Panel>

</asp:Content>

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



WordAutomationConverter.aspx.cs

Read the input file in the Page_Load event

        <span class="kwrd">protected</span> <span class="kwrd">void</span> Page_Load(<span class="kwrd">object</span> sender, EventArgs e)
        {
            <span class="rem">// Only set up the values the first time the page is loaded</span>
            <span class="kwrd">if</span> (!IsPostBack)
            {
                <span class="rem">// Get the input file URL from the query string</span>
                <span class="kwrd">string</span> itemUrl = Request.QueryString[<span class="str">"ItemUrl"</span>];
                <span class="kwrd">string</span> inputPath = Site.Url + itemUrl;
                lblInputItem.Text = inputPath;
            }
        }

<style type="text/css"> .csharpcode, .csharpcode pre { font-size: small; color: black; font-family: consolas, "Courier New", courier, monospace; background-color: #ffffff; /*white-space: pre;*/ } .csharpcode pre { margin: 0em; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt { background-color: #f4f4f4; width: 100%; margin: 0em; } .csharpcode .lnum { color: #606060; }</style>Convert the document 

        <span class="kwrd">protected</span> <span class="kwrd">void</span> btnConvertSharePointFile_Click(<span class="kwrd">object</span> sender, EventArgs e)
        {
            WordAutomationConverterHelper converter = <span class="kwrd">new</span> WordAutomationConverterHelper(SPContext.Current.Site);
            <span class="kwrd">string</span> inputFile = lblInputItem.Text;
            <span class="kwrd">string</span> outputFile = Path.ChangeExtension(lblInputItem.Text, <span class="str">"pdf"</span>);

            <span class="rem">//Conversion takes place here</span>
            var conversionStatus = converter.ConvertWordDocument(inputFile, outputFile);
            var conversionTime = conversionStatus.CompleteTime.Value.Subtract(conversionStatus.StartTime.Value).Milliseconds.ToString();

            <span class="kwrd">if</span> (conversionStatus.Succeeded)
            {
                lblConvertStatus.Text = <span class="kwrd">string</span>.Format(<span class="str">"File converted from <a href='{0}'>{0}</a> to <a href='{1}'>{1}</a>. in {2} milliseconds"</span>, inputFile, outputFile, conversionTime);
            }
            <span class="kwrd">else</span>
            {
                lblConvertStatus.Text = <span class="kwrd">string</span>.Format(<span class="str">"Something went wrong while converting with message: {0}"</span>, conversionStatus.ErrorMessage);
            }
            pnlConversion.Visible = <span class="kwrd">false</span>;
            pnlCompleted.Visible = <span class="kwrd">true</span>;
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



Download the document

        <span class="kwrd">protected</span> <span class="kwrd">void</span> btnDownloadAsPdf_Click(<span class="kwrd">object</span> sender, EventArgs e)
        {
            WordAutomationConverterHelper converter = <span class="kwrd">new</span> WordAutomationConverterHelper(SPContext.Current.Site);
            <span class="kwrd">string</span> inputFile = lblInputItem.Text;

            <span class="kwrd">byte</span>[] file;
            SPFile document = SPContext.Current.Web.GetFile(lblInputItem.Text);

            var conversionStatus = converter.ConvertWordDocument(document.OpenBinary(), <span class="kwrd">out</span> file);
            <span class="kwrd">if</span> (conversionStatus.Succeeded)
            {
                lblConvertStatus.Text = <span class="kwrd">string</span>.Format(<span class="str">"File converted successfully."</span>);
                pnlConversion.Visible = <span class="kwrd">false</span>;
                pnlCompleted.Visible = <span class="kwrd">true</span>;

                Response.Buffer = <span class="kwrd">false</span>; <span class="rem">//transmitfile self buffers</span>
                Response.Clear();
                Response.ClearContent();
                Response.ClearHeaders();
                Response.ContentType = <span class="str">"application/pdf"</span>;
                Response.AddHeader(<span class="str">"Content-Disposition"</span>, <span class="str">"attachment; filename="</span> + Path.ChangeExtension(document.Name, <span class="str">"pdf"</span>));   
                Response.BinaryWrite(file);
                Response.End();
            }
            <span class="kwrd">else</span>
            {
                lblConvertStatus.Text = <span class="kwrd">string</span>.Format(<span class="str">"Something went wrong while converting with message: {0}"</span>, conversionStatus.ErrorMessage);
            }
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



WordAutomationConverterHelper

    <span class="kwrd">public</span> <span class="kwrd">class</span> WordAutomationConverterHelper
    {
        <span class="kwrd">private</span> SPSite _site;

        <span class="kwrd">public</span> WordAutomationConverterHelper(SPSite site)
        {
            _site = site;
        }

        <span class="rem">/// <summary></span>
        <span class="rem">/// Converts the word document.</span>
        <span class="rem">/// </summary></span>
        <span class="rem">/// <param name="inputPath">The input path.</param></span>
        <span class="rem">/// <param name="outPutPath">The out put path.</param></span>
        <span class="rem">/// <returns>Conversion Status.</returns></span>
        <span class="kwrd">public</span> ConversionItemInfo ConvertWordDocument(<span class="kwrd">string</span> inputPath, <span class="kwrd">string</span> outPutPath) 
        {
            SPServiceContext context = SPServiceContext.GetContext(_site);

            <span class="rem">//Get default WordService application</span>
            WordServiceApplicationProxy wordProxy = context.GetDefaultProxy(<span class="kwrd">typeof</span>(WordServiceApplicationProxy)) <span class="kwrd">as</span> WordServiceApplicationProxy;

            <span class="rem">//Set the settings for the conversion</span>
            ConversionJobSettings jobSettings = <span class="kwrd">new</span> ConversionJobSettings();
            <span class="rem">//SaveFormat.Automatic will try to select the format based on the extension of outPuthPath</span>
            jobSettings.OutputFormat = SaveFormat.Automatic;

            SyncConverter converter = <span class="kwrd">new</span> SyncConverter(wordProxy, jobSettings);
            converter.UserToken = _site.RootWeb.CurrentUser.UserToken;

            ConversionItemInfo result = converter.Convert(inputPath, outPutPath);
            <span class="kwrd">return</span> result;
        }


        <span class="rem">/// <summary></span>
        <span class="rem">/// Converts the byte array to PDF.</span>
        <span class="rem">/// </summary></span>
        <span class="rem">/// <param name="inputFile">The input file byte array.</param></span>
        <span class="rem">/// <param name="outPutFile">The output file as byte array.</param></span>
        <span class="rem">/// <returns>Conversion Status.</returns></span>
        <span class="kwrd">public</span> ConversionItemInfo ConvertWordDocument(<span class="kwrd">byte</span>[] inputFile, <span class="kwrd">out</span> <span class="kwrd">byte</span>[] outPutFile)
        {
            SPServiceContext context = SPServiceContext.GetContext(_site);

            <span class="rem">//Get default WordService application</span>
            WordServiceApplicationProxy wordProxy = context.GetDefaultProxy(<span class="kwrd">typeof</span>(WordServiceApplicationProxy)) <span class="kwrd">as</span> WordServiceApplicationProxy;

            <span class="rem">//Set the settings for the conversion</span>
            ConversionJobSettings jobSettings = <span class="kwrd">new</span> ConversionJobSettings();
            jobSettings.OutputFormat = SaveFormat.PDF;

            SyncConverter converter = <span class="kwrd">new</span> SyncConverter(wordProxy, jobSettings);
            converter.UserToken = _site.RootWeb.CurrentUser.UserToken;

            <span class="kwrd">byte</span>[] convertedFile;
            ConversionItemInfo result = converter.Convert(inputFile, <span class="kwrd">out</span> convertedFile);
            outPutFile = convertedFile;

            <span class="kwrd">return</span> result;
        }
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



## Summary

With the possibility for on demand conversions it is now possible to interact immediately with your users and make it possible to create ad-hoc conversions of your word documents.

