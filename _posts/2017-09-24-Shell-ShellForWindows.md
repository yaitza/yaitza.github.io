---
layout: post
category : 工具
title: Shell For Windows
tagline: Shell
tags: Shell
---




# 1. windows上unix工具

&nbsp;&nbsp;&nbsp;&nbsp;工作中经常会使用一些命令来执行一些操作，由于Windows的命令功能没Linux命令功能强大，且在两个操作系统之间切换，使用命令也容易搞混淆；于是就发现了一款可以在Windows上使用Linux或Unix命令的工具。   

&nbsp;&nbsp;&nbsp;&nbsp;**UnxUtils** is a collection of ports of common GNU Unix-like utilities to native Win32, with executables only depending on the Microsoft C-runtime msvcrt.dll. The collection was last updated externally on April 15, 2003 by Karl M. Syring. The most recent release package available as of December 2016 was an open-source project, UnxUtils at SourceForge, with the latest binary release in March, 2007 (though the files within are dated from the year 2000). The independent distribution included a main zip archive (UnxUtils.zip, 3,365,638 bytes) complemented by more recent updates (UnxUpdates.zip, 878,847 bytes, brought some binaries up to year 2003), but the Sourceforge project has no UnxUpdates.zip package. An alternative source of Unix-like utilities for Windows is GnuWin32; it has later versions of many programs, but requires supporting files (e.g. DLLs) in many cases.

# 2. UnxUtils安装

&nbsp;&nbsp;&nbsp;&nbsp;按照提供的[DownLoad Address](https://sourceforge.net/projects/unxutils/)下载UnxUtils，解压UnxUtils得到：
![pic1](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Tools/2017-09-24-Shell-ShellForWindows-1.png)  

&nbsp;&nbsp;&nbsp;&nbsp;然后将D:\UnxUtils\usr\local\wbin设置于Windows操作系统环境变量Path中，然后就可以像Linux或Unix一样使用相关命令了。
