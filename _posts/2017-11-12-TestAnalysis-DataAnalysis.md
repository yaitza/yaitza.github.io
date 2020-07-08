---
layout: post
category : 测试
title: 测试数据分析
tagline: Test Analysis
tags: Test
---

&nbsp;&nbsp;&nbsp;&nbsp;由于公司做了一个用于局域网交互的系统，此系统包含的设备包括VR设备(类似于手机)，平板电脑，电脑。主要通过电脑作为服务器，将相关课程数据(类似于视频，apk等)推送至VR设备，且能够发送一些指令对VR设备进行操控。当然平板电脑也能够作为控制所有VR设备的一个终端。这套系统中电脑一台，平板电脑一台，VR设备N台，就这样的一套系统，需要对其局域网的稳定性，以及在线率等进行测试，给出数据。

&nbsp;&nbsp;&nbsp;&nbsp;电脑端有一个控制台，可以查看VR端以及平板电脑的各种状态，如下图。当然我，首先需要搭一个这样的局域网环境，很简单就是找个路由器，连接到路由器上就可以了，登陆服务端网站就能查看到对应设备是否在线。  
![pic1](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/1-2017-11-12-TestAnalysis-DataAnalysis.png) 

&nbsp;&nbsp;&nbsp;&nbsp;要看设备是否保持在线，当然是通过上图的网站来查看设备是否在线；可是我不可能实时来刷新网站，并手动记录数据；于是这里就通过服务端开的接口对接口(http://localhost:8100/api/device)进行请求，并通过返回的json数据进行分析，保存记录下设备在线数据。

&nbsp;&nbsp;&nbsp;&nbsp;https://github.com/yaitza/RequestAnalysis

&nbsp;&nbsp;&nbsp;&nbsp;通过上面工具记录下来的数据，就是最原始的数据；需要对原始数据进行各种分析，得到想要的结果；可是有的时候数据量很大，而且很繁琐；这时候无意中发现Excel，宏录制功能很好用，就录制了一段宏；处理十来个Excel数据，感觉还是很方便。可是突然有一天Excel上升到50个了，我再一个一个点开，尼玛就有点不现实了，于是我又开始了些VBA脚本了，写完之后发现功能很强大，以后可以节省好多工作。

	Sub ExecuteAllFiles()
    '统计当前文件夹下所有csv文件的测试数据
    
	    Dim DefaultPath, logFilePath As String
	    Dim fileDir(100) As String
	    
	    DefaultPath = Application.ActiveWorkbook.Path & "\"
	    logFilePath = DefaultPath & "NoCount_OffLine_Count.csv"
	
	    Dim counter As Long
	    counter = Range("S3").Value
	    If counter = 0 Then
	        Range("S3").Select
	        MsgBox ("请在选中的单元格输入需要统计数据的行数。" & vbCrLf & "输入行数以Excel的行标记为准。 ")
	    ElseIf Dir(logFilePath) = "" & Range("S9") = 1 Then
	        MsgBox ("对应目录中无Warn.log日志文件统计数据。")
	    Else
	        '设置文档首行标题
	        Cells(1, "A") = "SN号"
	        Cells(1, "B") = "掉线次数"
	        Cells(1, "C") = "掉线时长"
	        Cells(1, "D") = "设备对应IP"
	        
	        If Range("S9") = 1 Then
	            Cells(1, "E") = "5s掉线1次"
	            Cells(1, "F") = "5s掉线2次"
	            Cells(1, "G") = "5s掉线3次"
	        End If
	
	        filePath = Dir(DefaultPath & "")
	        
	        Dim iCounter As Integer
	        iCounter = 2
	
	        '遍历当前目录下所有文件
	        While filePath <> ""
	            If Left(filePath, 8) <> "NoCount_" Then
	                Cells(iCounter, "A").NumberFormatLocal = "@"
	                Cells(iCounter, "A") = Left(filePath, 15)
	                fileDir(iCounter - 2) = DefaultPath & filePath
	                
	                Application.Workbooks.Open (fileDir(iCounter - 2))
	                Dim calData, offLineCounts, offLineSeconds, deviceIp As String
	                calData = CountTestData(Application.ActiveSheet, 2, counter)
	                offLineCounts = Split(calData, "|")(1)
	                offLineSeconds = Split(calData, "|")(0)
	                deviceIp = Split(calData, "|")(2)
	                Application.ActiveWorkbook.Close SaveChanges:=0
	
	                Cells(iCounter, "B") = offLineCounts
	                Cells(iCounter, "C") = offLineSeconds
	                Cells(iCounter, "D") = deviceIp
	
	                If Range("S9") = 1 Then
	                    Application.Workbooks.Open (logFilePath)
	                    secondCount = CountWarnOffLinebyfiveSeconds(Application.ActiveSheet, deviceIp)
	                    Application.ActiveWorkbook.Close SaveChanges:=0
	                    
	                    Cells(iCounter, "E") = Split(secondCount, "|")(0)
	                    Cells(iCounter, "F") = Split(secondCount, "|")(1)
	                    Cells(iCounter, "G") = Split(secondCount, "|")(2)
	                End If
	                iCounter = iCounter + 1
	            End If
	            filePath = Dir
	        Wend
	    End If
	End Sub

&nbsp;&nbsp;&nbsp;&nbsp;这段脚本中，对于遍历文件夹下所有文件的处理，肯定以后还会用到，此处特定说明。

    Function TraverseFolder(folderPath As String)
		folderDir = folderPath & "\"
		filePath = Dir(folderDir  "")
		while filePath <> ""
			Debug.Print(filePath)
			filePath = Dir
		Wend
	End Function

&nbsp;&nbsp;&nbsp;&nbsp;工具示例Demo：[Demo Download](./resource/Test/2017-11-12-TestAnalysis-DataAnalysis.rar)