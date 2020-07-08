---
layout: post
category: 编程
title: 筛选指定尺寸的图片
tagline: python
tag: python
---

筛选出指定文件夹下指定尺寸的图片，需要用到python [PIL库](http://www.pythonware.com/products/pil/).  
**注：**  

- 在安装PIL库的过程中碰到过python未注册的情况，提示：`python version 2.7 required,which was not found in the registry.`  
- 处理办法,本地执行如下代码 [Register](./resource/Programing/Register.py)  

通过如下代码获取对应图片尺寸：
{% highlight python %}
from PIL import Image

img = Image.open(self.pic_path)
print img.size  
{% endhighlight %}

通过遍历文件夹下所有图片尺寸，筛选出符合要求的图片.

代码示例:

{% highlight python %}
#!/usr/bin/python
# coding=utf-8
# Copyright 2017 yaitza. All Rights Reserved.
#
#     https://yaitza.github.io/
#
# My Code hope to usefull for you.
# ===================================================================

__author__ = "yaitza"
__date__ = "2017-04-26 13:59"

import os
from PIL import Image

class ImageHandler:
    def __init__(self, pic_path):
        self.pic_path = pic_path

    def getPicSize(self, maxSize, minSize):
        try:
            img = Image.open(self.pic_path)
        except IOError:
            print self.pic_path + " Error!"
            return
        if max(img.size) <= maxSize and max(img.size) >= minSize and min(img.size) >= minSize and min(img.size) <= maxSize:
            return img.size
        return

class FileHandler:
    def __init__(self, file_path):
        self.file_path = file_path

    def getAllFiles(self):
        fileList = os.listdir(self.file_path)
        file_dir = []
        for file in fileList:
            file_dir.append(self.file_path + "/" + file)
        useFileList = []
        for file in file_dir:
            im = ImageHandler(file)
            if im.getPicSize(1204, 480) is not None:
                useFileList.append(file)
        return useFileList

if __name__ == "__main__":
    file_path = "E:/内容素材/图片/美女图"
    uipath = unicode(file_path, "utf8")
    fh = FileHandler(uipath)
    fh.getAllFiles()  
{% endhighlight %}