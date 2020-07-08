---
layout: post
category: 编程
title: TestLinkConverter编程纪要
tagline: C#
tag: C#
---

&nbsp;&nbsp;&nbsp;&nbsp;TestLinkConverter小工具主要是为了解决XML以及Excel之间相互转换的问题。测试管理工具导出用例文件格式为XML，而一般测试人员习惯用Excel设计测试用例，导致设计的测试用例无法导入到TestLink以及TestLink导出的测试用例无法提供Excel视图;支持XML转换为Excel后，对应Excel转换为XML；XML和Excel相互转换。  
![1-2017-05-21-CSharp-TestLink](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Programing/1-2019-09-18-CSharp-TestLink.png)  
**[点击下载](https://github.com/yaitza/TestLinkConverter/releases)**

# 1. 概要
&nbsp;&nbsp;&nbsp;&nbsp;支持解析的XML为TestLink默认导出的XML结构；经过工具生成后对应Excel展示用例如下图：  
![2-2017-05-21-CSharp-TestLink](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Programing/2-2019-09-18-CSharp-TestLink.png)  
&nbsp;&nbsp;&nbsp;&nbsp;支持解析的Excel格式默认为导出的Excel格式，见上图。  

**注：** 支持导出文件的相互转换，即导出XML再转为Excel或者导出Excel再次转为XML。

# 2. XML -> Excel
&nbsp;&nbsp;&nbsp;&nbsp;首先解决的问题是如何将TestLink导出的XML文件进行转换提供给测试人员习惯使用的Excel视图。一般的XML格式TestLink一般导出后结构如下，对应XML各个字段定义参考[TestLink字段解析](http://wangsx.cn/?p=648)：  
![4-2017-05-21-CSharp-TestLink](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Programing/4-2019-09-18-CSharp-TestLink.png)  
TestLinkConverter采用C#自带的XmlDocument对xml文件进行解析，但是有的xml可能存在多个testsuite嵌套的情况，就需要获取所有嵌套下的TestCase，这里采用递归的方法获取所有testcase。

```c#
/// <summary>
/// 递归获取测试套下所有测试用例
/// </summary>
/// <param name="xmlNodes">根节点下子节点集合</param>
private void RecursionGetNodes(List<XmlNode> xmlNodes)
{
    foreach (XmlNode node in xmlNodes)
    {
        if (node.Name.Equals("testsuite"))
        {
            List<XmlNode> childNodes = node.ChildNodes.Cast<XmlNode>().ToList();
            RecursionGetNodes(childNodes);
        }
        else
        {
           if (node.Name.Equals("testcase"))
           {
              this._nodesList.Add(node);
           }
        }
     }
}
```



# 3. Excel -> XML

# 4. 功能实现
## 4.1 进度条功能  
对应进度条的功能，首先不确定解析文件以及生成文件具体耗时，进度条而且需要与之同步；进度条采用系统自带的ProgressBar控件。  

![7-2017-05-21-CSharp-TestLink](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Programing/7-2017-05-21-CSharp-TestLink.gif)   

```c#
private delegate void SetPos(int ipos);

public Form()
{
    InitializeComponent();    
    ProgressBarShow.SetProgressValue += this.SetProgressValue;
}

private void SetProgressValue(int ipos)
{
    if (this.InvokeRequired)
    {
        SetPos setPos = new SetPos(SetProgressValue);
        this.Invoke(setPos, new object[] {ipos});
    }
    else
    {
        this.progressBar.Value = Convert.ToInt32(ipos);
    }
}
```

新增类用于绑定进度条实时更新。

```c#
public static class ProgressBarShow
{
    public delegate void SetProgressHandler(int ipos);

    public static SetProgressHandler SetProgressValue { get; set; }

    public static void ShowProgressValue(int ipos)
    {
        if (SetProgressValue != null)
        {
            SetProgressValue.Invoke(ipos);
        }
    }
}
```


# 5. 引用三方类库
## 5.1 log4net
**下载地址：**  <https://logging.apache.org/log4net/download_log4net.cgi>

**参考博客：**  <http://www.cnblogs.com/kissazi2/p/3389551.html>  

# 6. 问题    
代码过程中以及环境设置中出现的相关问题。
## 6.1. 调用Excel相关类库时环境配置问题
**问题描述：**  
无法嵌入互操作类型“Microsoft.Office.Interop.Word.ApplicationClass”。请改用适用的接口。
错误 4317 无法嵌入互操作类型“Microsoft.Office.Interop.Word.ApplicationClass”。请改用适用的接口。
类型“Microsoft.Office.Interop.Word.ApplicationClass”未定义构造函数。  
**解决办法：**  
在Visual Studio 中点击菜单项“视图->解决方案资源管理器”，在其中点开“引用”文件夹，在"Microsoft.Office.Interop.Excel;" 上点击鼠标右键，选择“属性”，将属性中的“嵌入互操作类型”的值改为“false”即可。  
**说明:**  
引用Excel类库：Microsoft Excel 14.0 Object Library，需要安装Office 2010.    
![5-2017-05-21-CSharp-TestLink](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Programing/5-2017-05-21-CSharp-TestLink.png)  
## 6.2 系统类库跟随解决方案生成至本地
**问题描述：**  
代码运行打包后运行的环境可能没有安装对应版本的Office，而程序引用的Office相关的类库版本较高，可能导致不兼容的情况；于是把程序引用的所有类库包括系统类库均打包至本地。  
**解决办法：**  
只需要将接近方案中对应DLL的应用属性“复制到本地”设置为True。  
![6-2017-05-21-CSharp-TestLink](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Programing/6-2017-05-21-CSharp-TestLink.png)