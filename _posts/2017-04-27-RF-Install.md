---
layout: post
category: 工具
title: Robot Framwork环境搭建
tagline: Robot Framework
tag: Robot Framework
---

&nbsp;&nbsp;&nbsp;&nbsp;工作中接触到Robot Framework这个测试框架，突然发现其既可以进行UI自动化测试也可以进行接口自动化测试；发现真是一个不错的测试框架，于是就把学习的过程记录下来，希望对阅读到这片文章的人有用。  

# 1. Robot Framwork环境准备  
- 1.[Python](https://www.python.org/downloads/)  
python安装过程中记得勾选将python加入环境变量PATH的选项。添加环境变量成功后，在windows cmd命令窗口下输入python会有如下图信息显示。  
![提示信息](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Tools/1-2017-04-27-RF-Install.png)   
正常情况Windows对应python的安装包均带有如下两个模块，如没有请自行安装如下类库。 
	- [SetupTools](https://pypi.python.org/pypi/setuptools)  
	- [pip](https://pypi.python.org/pypi/pip/)
- 2.[Robot Framework](https://github.com/robotframework/robotframework/releases)  
安装方法：解压缩tar.gz文件，在cmd命令行窗口进入解压出来的目录，输入```python setup.py install```,然后按回车键等待安装完成。或者直接运行```pip install robotframework```。  
![pip install RobotFramework](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Tools/2-2017-04-27-RF-Install.png)  
- 3.[wxPython](https://wxpython.org/download.php)  
wxPython用于支持Python图形化界面，安装它主要是用来运行RIDE。  
官方下载也罪行微信Python版本已迭代至3.0.2.0版本，但是RIDE本身并未说明是否支持3.0版本；但是安装wxPython 3.0.2.0版本后运行RIDE后提示如下信息：  
![Ride Error](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Tools/4-2017-04-27-RF-Install.png)  
于是只有按照提示信息安装2.8.12.1版本wxPython。
- 4.[robotframework-ride](https://github.com/robotframework/RIDE/releases)  
安装方法：解压缩tar.gz文件，在cmd命令行窗口进入解压出来的目录，输入```python setup.py install```,然后按回车键等待安装完成。或者直接运行```pip install robotframework-ride```。  
![pip install RobotFramework-ride](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Tools/3-2017-04-27-RF-Install.png)
  
运行如下脚本：  
![ride start](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Tools/5-2017-04-27-RF-Install.png)  
RIDE图像界面如下图显示，代表Robot Framework环境搭建完成。  
![ride](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Tools/6-2017-04-27-RF-Install.png) 
# 2.设置图标显示 
- 新建->快捷方式；
- "请键入对象位置(T):"输入命令```C:\Python27\pythonw.exe -c "from robotide import main; main()"```；  
![ride](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Tools/7-2017-04-27-RF-Install.png) 
- "键入快捷方式的名称(T):"输入```RIDE```；   
- 双击图标将弹出RIDE主界面。
```当然可以更改快捷方式的图标，RIDE提供图标目录C:\Python27\Lib\site-packages\robotide\widgets\robot.ico```  
![ride](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Tools/8-2017-04-27-RF-Install.png) 

