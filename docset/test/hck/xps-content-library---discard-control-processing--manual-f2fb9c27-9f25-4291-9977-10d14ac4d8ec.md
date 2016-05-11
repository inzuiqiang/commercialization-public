---
author: joshbax-msft
title: XPS Content Library - Discard Control Processing (MANUAL)
description: XPS Content Library - Discard Control Processing (MANUAL)
MSHAttr:
- 'PreferredSiteName:MSDN'
- 'PreferredLib:/library/windows/hardware'
ms.assetid: 0a0b18c1-5e3b-4216-8dd6-8ad77c862db5
---

# XPS Content Library - Discard Control Processing (MANUAL)


This manual test loads both valid and invalid XPS documents that contain discard control features, and then prints the documents to the specified printer. This test only has to be run if the consumer supports discard control processing. This test uses the XPS Content Library tool.

By using this test tool, you can generate jobs to send to a printer or to XPS serialization. This tool includes options for validating the XPS that the tool produces, or for selecting particular XML files. The tool exits after the tool prints each file one time.

## Test details


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Associated requirements</strong></p></td>
<td><p>Device.Imaging.Printer.XPS.XPS</p>
<p>[See the device hardware requirements.](http://go.microsoft.com/fwlink/p/?linkid=254483)</p></td>
</tr>
<tr class="even">
<td><p><strong>Platforms</strong></p></td>
<td><p>Windows 7 (x64) Windows 7 (x86) Windows 8 (x64) Windows 8 (x86) Windows Server 2012 (x64) Windows Server 2008 R2 (x64) Windows 8.1 x64 Windows 8.1 x86 Windows Server 2012 R2</p></td>
</tr>
<tr class="odd">
<td><p><strong>Expected run time</strong></p></td>
<td><p>~2 minutes</p></td>
</tr>
<tr class="even">
<td><p><strong>Categories</strong></p></td>
<td><p>Certification Functional</p></td>
</tr>
<tr class="odd">
<td><p><strong>Type</strong></p></td>
<td><p>Manual</p></td>
</tr>
</tbody>
</table>

 

## Running the test


Before you run the test, complete the test setup as described in the test requirements: [Printer Testing Prerequisites](printer-testing-prerequisites.md).

In addition, this test requires the following:

-   Printer paper

## Troubleshooting


For troubleshooting information, see [Troubleshooting Device.Imaging Testing](troubleshooting-deviceimaging-testing.md).

## More information


The XPS Content Library tool prints a set of XPS file content that is a representative subset of the XPS specification. The tool uses a static set of XPS files.

An XPS print filter or device must consume all the valid XPS test files successfully and fail gracefully (that is, no crashes, "hangs," or resource leaks may occur) on the invalid XPS files.

This job is similar to the standard XPS Content Library job. However, this job focuses only on functionality that relates to Discard Control processing. The job only appears in the submission if the user has specified that the test printer supports Discard Control processing.

This job must only print one page. The page must be titled "Valid Discard Control" and contain a multi-colored circle inside a black square box, as seen in the following image:

![image of a multi-color circle in a black square](images/hck-win8-xps-content-library-discard-control-processing.png)

The following documents relate to Discard Control processing:

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th>Document name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>M10.6a</p></td>
<td><p>DiscardControl references itself (TargetUri)</p></td>
</tr>
<tr class="even">
<td><p>M10.6b</p></td>
<td><p>DiscardControl references itself (SentinelUri)</p></td>
</tr>
<tr class="odd">
<td><p>M10.23a</p></td>
<td><p>Multiple DiscardControl package relationships</p></td>
</tr>
<tr class="even">
<td><p>DiscardControl.xps</p></td>
<td><p>Valid XPS file that contains a discard control</p></td>
</tr>
</tbody>
</table>

 

 

 





