---
layout: post
category: 工具
title: Pycharm修改文件默认生成
tagline: Pycharm
tag: 工具
---
# 1. Pycharm设置路径
pycharm 修改新建文件时的头部模板:  
  >默认为__author__ = "..." [省略号是默认你的计算机名]. 
  
修改这个作者名的步骤：  
  >1.依次点击：File->Settings->Editor->File and Code Templete.  
  >2.点击右侧Templates选项卡，会有一些格式文件新建时的模板.   
  >3.在这里可以修改这些默认模板.  

# 2. Python Script示例
按照如下设置进行Python Script默认指设置：  
![Pycharm截图](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Tools/1-2017-04-20-Pycharm-Setting.png)  
设置完成后，重启Pycharm；新建Python Script查看，如下图：  
![Pycharm截图](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Tools/2-2017-04-20-Pycharm-Setting.png)
# 3. 常用替换变量  
如上图所示， 也可以把${USER} 换成你想要的作者名；也可以换成别的变量，或其他代码：  
{% highlight Console%}
${PROJECT_NAME} - the name of the current project.  
${NAME} - the name of the new file which you specify in the New File dialog box during the file creation.  
${USER} - the login name of the current user.  
${DATE} - the current system date.  
${TIME} - the current system time.  
${YEAR} - the current year.  
${MONTH} - the current month.  
${DAY} - the current day of the month.  
${HOUR} - the current hour.  
${MINUTE} - the current minute.  
${PRODUCT_NAME} - the name of the IDE in which the file will be created.  
${MONTH_NAME_SHORT} - the first 3 letters of the month name. Example: Jan, Feb, etc.  
${MONTH_NAME_FULL} - full name of a month. Example: January, February, etc.
{% endhighlight %}