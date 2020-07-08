---
layout: post
category: 编程
title: OpenFileDialog和SaveFileDialog使用
tagline: C#
tag: C#
---

# 1. 概要
&nbsp;&nbsp;&nbsp;&nbsp;写一个小工具实现文件的浏览，奈何很久没有碰过C#了，导致相关组件使用都忘掉了。今天借此机会补充起来，OpenFileDialog和SaveFileDialog的运用。

# 2. OpenFileDialog
&nbsp;&nbsp;&nbsp;&nbsp;使用OpenFileDialog主要想实现如下图的功能.  

![OpenFileDialog](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Programing/1-2017-05-17-CSharp-OpenFileDialogAndSaveFileDialog.gif)  

## 2.1 添加控件
&nbsp;&nbsp;&nbsp;&nbsp;在设计页面直接添加对话框组件；然后设置其对应属性.  

![control](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Programing/2-2017-05-17-CSharp-OpenFileDialogAndSaveFileDialog.png)  

&nbsp;&nbsp;&nbsp;&nbsp;按照设计需要进行相关属性设置.   

![Setting](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Programing/3-2017-05-17-CSharp-OpenFileDialogAndSaveFileDialog.png) 

&nbsp;&nbsp;&nbsp;&nbsp;然后按照需求添加一个Button控件，并绑定该控件的Click事件，实现代码。

{% highlight C# lineno %}
private void getFilePathBtn_Click(object sender, EventArgs e)
{
    if (openFileDialog.ShowDialog() == DialogResult.OK)
    {
        filePathTb.Text = openFileDialog.FileName;
    }
}
{% endhighlight %}

## 2.2 代码实现  
&nbsp;&nbsp;&nbsp;&nbsp;代码实现只是将属性设置的模块进行代码设置而已。依旧需要添加Button控件，并绑定该控件Click事件，并在该事件内实现代码。

{% highlight C# lineno %}
private void getFilePathBtn_Click(object sender, EventArgs e)
{
	OpenFileDialog openFileDialog = new OpenFileDialog();
    openFileDialog.InitialDirectory = "c:\\";//注意这里写路径时要用c:\\而不是c:\
    openFileDialog.Filter = "xml(*.xml)|*.xml|Excel 2003(*.xls)|*.xls|Excel 2007Plus(*.xlsx)|*.xlsx";
    openFileDialog.RestoreDirectory = true;
    openFileDialog.FilterIndex = 1;
    if (openFileDialog.ShowDialog() == DialogResult.OK)
    {
        filePathTb.Text = openFileDialog.FileName;
    }
}
{% endhighlight %}