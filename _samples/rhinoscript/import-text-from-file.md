---
title: Import Text from File
description: Demonstrates how to import text from a file using RhinoScript.
authors: ['Dale Fugier']
author_contacts: ['dale']
apis: ['RhinoScript']
languages: ['VBScript']
platforms: ['Windows']
categories: ['Other']
origin: http://wiki.mcneel.com/developer/scriptsamples/importtext
order: 1
keywords: ['rhinoscript', 'vbscript']
layout: code-sample-rhinoscript
---

```vbnet
Sub ImportText

  Const ForReading = 1
  Dim strFile, strText
  Dim objFSO, objFile
  Dim arrOrigin

  strFile = Rhino.OpenFileName("Open", "Text Files (*.txt)|*.txt|")
  If IsNull(strFile) Then Exit Sub

  arrOrigin = Rhino.GetPoint("Start point")
  If Not IsArray(arrOrigin) Then Exit Sub

  Set objFSO = CreateObject("Scripting.FileSystemObject")

  On Error Resume Next
  Set objFile = objFSO.OpenTextFile(strFile, ForReading)
  If Err Then
    MsgBox Err.Description
    Exit Sub
  End If

  While Not objFile.AtEndOfStream
    strText = strText & objFile.ReadLine
    If Not objFile.AtEndOfStream Then
      strText = strText & VbCrLf
    End If
  Wend

  objFile.Close

  Set objFile = Nothing
  Set objFSO = Nothing

  If Len(strText) > 0 Then
    Rhino.AddText strText, arrOrigin
  End If

End Sub
```
