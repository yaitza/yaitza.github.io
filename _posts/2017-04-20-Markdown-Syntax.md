---
layout: post
category : 编程
title: Markdown语法
tagline:
tags : 编程
---

本文作为一个Markdown语法的一个参考手册，列出Markdown支持的语法。
Markdown的语法标记是一些常见的特殊字符，譬如 **\#, \*, \>** 等，使用起来都比较自然。

<br/><font color='blue' size='5'><b>标题</b></font>

	# 一级标题
    ## 二级标题
    ##### 五级标题

![各级标题](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Programing/1-2017-04-20-Markdown-Syntax.png)

<br/><font color='blue' size='5'><b>强调</b></font>

	*斜体* 或 _斜体_
    **粗体** 或 __粗体__
	~~删除线~~
	**_加粗且斜体_**

*斜体*

**粗体**

~~删除线~~

**_加粗且斜体_**

<br/><font color='blue' size='5'><b>列表</b></font>

	* 一级表项
	* 一级表项
	    * 二级表项
	    * 二级表项

    1. 一级表项1
	    * 二级表项1
	    * 二级表项2
    2. 一级表项2


* 一级表项
    * 二级表项
    * 二级表项
* 一级表项

1. 一级表项1
    * 二级表项1
    * 二级表项2
2. 一级表项2


<br/><font color='blue' size='5'><b>引用</b></font>

	毛主席说过:

    > 枪杆子里出政权！
    >
    > 一切反动派都是纸老虎。

毛主席说过：

> 枪杆子里出政权！
>
> 一切反动派都是纸老虎。


<br/><font color='blue' size='5'><b>链接</b></font>

	自动链接 <https://yaitza.github.io>
    文本链接 [Click to yaitza](https://yaitza.github.io)

自动链接 <https://yaitza.github.io>

文本链接 [Click to yaitza](https://yaitza.github.io)


<br/><font color='blue' size='5'><b>图片</b></font>

	![文字描述](图片链接)
    ![锤锤](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Programing/2-2017-04-20-Markdown-Syntax.png)

![文字描述](图片链接)

![锤锤](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Programing/2-2017-04-20-Markdown-Syntax.png)


<br/><font color='blue' size='5'><b>表格</b></font>

	head1 | head2
    ----- | -----
     1.1  |  1.2
     2.1  |  2.2


head1 | head2
----- | -----
 1.1  |  1.2
 2.1  |  2.2


<br/><font color='blue' size='5'><b>转义</b></font>

`\`是转义前导符，能够还原特殊字符本来的样子。

	\#, \*, \>, \{}, \[], \(), \\, \+, \-, \`, \_, \!, \.

\#, \*, \>, \{}, \[], \(), \\, \+, \-, \`, \_, \!, \.


<br/><font color='blue' size='5'><b>任务列表</b></font>

不同的Markdown对任务列表的支持程度不一样。

	- [x] 任务一，已完成
	- [ ] 任务二，未完成
	- [ ] 任务三，未完成


<div ><img src="https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Programing/3-2017-04-20-Markdown-Syntax.png" alt="任务列表"/></div>

<br/><font color='blue' size='5'><b>代码块语法高亮</b></font>

代码块语法高亮需要一些插件的支持，不同的插件使用略有差别。

	```java
    public static void main(String[] args) {
        System.out.println("hello, markdown");
    }
	```

{% highlight java %}
public static void main(String[] args) {
    System.out.println("hello, markdown");
}
{% endhighlight %}
