---
layout: post
category: 编程
title: PicRandShow编程纪要
tagline: C#
tag: C#
---

# PicRandShow
Picture Random Show 用于随机或者固定展示图片信息，并在展示过程中不断变换图片位置；主要用于人脸识别测试。

### Single Mode
![Single](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Programing/2017-08-20-CSharp-PicRandShow-1.gif)

### Multiple Mode
![Multiple](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Programing/2017-08-20-CSharp-PicRandShow-2.gif)

### Random Mode
![Random](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Programing/2017-08-20-CSharp-PicRandShow-3.gif)

## Settings
相关模式的设置主要采用配置文件的方式，对文件PicRandShow.exe.config进行相关设置，可以达到目前支持的三种效果展示。  
![Setting](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Programing/2017-08-20-CSharp-PicRandShow-4.png)

## Coding
主要采用容器Panel添加PictureBox实现的图片的展示的功能。

## ToDo
- 需要进一步了解委托，事件的相关原理以及实现方式。
- 需要了解线程的原理，以及实际实现细节。