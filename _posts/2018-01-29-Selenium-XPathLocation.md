---
layout: post
category : 测试
title: Selenium之XPath元素定位
tagline: Selenium
tags: Test
---

## 1. XPath节点 
 
&nbsp;&nbsp;&nbsp;&nbsp;XPath语言中提供了7种节点：文档节点(根节点)、元素、属性、文本、命名空间、处理指令、以及注释。XML文档被作为节点树对待，树的根被称作文档节点或根节点。

### 1.1 节点    

&nbsp;&nbsp;&nbsp;&nbsp;**XML实例文档**   

    <?xml version = "1.0" encoding = "utf-8" ?>  
    <!-- 这是一个注释节点-->  
    <booklist type = "kongfu">  
    	<book category = "kongfu">  
    		<title>葵花宝典</title>  
    		<author>东方不败</author>  
    		<pageNumber>400</pageNumber>  
    	</book>  
    </booklist>  

&nbsp;&nbsp;&nbsp;&nbsp;在上面的XML文档实例中展示的节点如下：   

	<booklist>:文档节点  
	<title>:元素节点   
	type = "kongfu":属性节点   

### 1.2 节点间的关系  

#### (1)父节点(Parent)  
&nbsp;&nbsp;&nbsp;&nbsp;每个元素以及属性都有一个父节点。上面的XML文档实例中，book元素是title、author以及pageNumber元素的父节点。  
#### (2)子节点(Children)  
&nbsp;&nbsp;&nbsp;&nbsp;一个元素节点可有零个，一个或多个子节点。上面的XML文档实例中，title、author以及pageNumber元素是book元素的子节点。  
#### (3)同胞节点(Sibling)  
&nbsp;&nbsp;&nbsp;&nbsp;同胞节点表示拥有相同父节点的节点。上面的XML文档实例中，title、author以及pageNumber元素都是同胞节点。  
#### (4)先辈节点(Ancestor)  
&nbsp;&nbsp;&nbsp;&nbsp;先辈节点表示的某节点的父节点，父节点的父节点，以及父节点的所有祖先节点。上面XML文档实例中，title元素的先辈节点有book和booklist。  
#### (5)后代节点(Descendant)  
&nbsp;&nbsp;&nbsp;&nbsp;后代节点表示某个节点的子节点，子节点的子节点，以及子节点的所有后代节点。上面的XML文档实例中，booklist元素的后代节点有book、title、author以及pageNumber元素。  

##  2. Xpath定位语法  
&nbsp;&nbsp;&nbsp;&nbsp;由于用于Web开发的的HTML语言的语法结构跟XML很相似，所以XPath也支持在HTML代码中定位HTML树状文档结构中的节点。  
&nbsp;&nbsp;&nbsp;&nbsp;**被测试网页的HTML代码：**   

	<html>
	    <body>
	        <div id="div1" style="text-align: center">
	            <img alt="dvi1-img1" 
	                 src="http://www.sougou.com/images/logo/new/sougou.png"
	                 href="http://www.sougou.com">搜狗图片</img><br />
	            <input name="div1input">
	            <a href="http://www.sougou.com">搜狗搜索</a>
	            <input type="button" value="查询"> 
	        </div>
	        <br>
	        <div id="div2" style="text-align: center">
	            <img alt="dvi2-img2"
	                 src="http://www.baidu.com/img/bdlogo.png"
	                 href="http://www.baidu.com">百度图片</img><br />
	            <input name="div1input">
	            <a href="http://www.baidu.com">百度搜索</a>
	            <input type="button" value="查询">
	        </div>
	    </body>
	</html>

&nbsp;&nbsp;&nbsp;&nbsp;使用上面的HTML代码生成被测试网页，基于此网页来实践各种不同页面元素的XPath定位方法。  
### 2.1 使用绝对路径来定位元素  
&nbsp;&nbsp;&nbsp;&nbsp;绝对路径表示页面元素在被测网页的HTML代码结构中，从根节点一层层地搜索到需要被定位的页面元素，绝对路径起始于正斜杠(/)，每一步均被斜杠分割。    
&nbsp;&nbsp;&nbsp;&nbsp;**目的：** 在被测网页中，查找第一个div标签下的“查询”按钮。  
&nbsp;&nbsp;&nbsp;&nbsp;**XPath定位表达式：** /html/body/div/input[@value="查询"]  
&nbsp;&nbsp;&nbsp;&nbsp;**Python定位语句：** query = driver.find_element_by_xpath('/html/body/div/input[@value="查询"]')  
&nbsp;&nbsp;&nbsp;&nbsp;**代码解释：**  
&nbsp;&nbsp;&nbsp;&nbsp;上述XPath定位表达式从HTML DOM树的根节点(html节点)开始逐层查找，最后定位到“查询”按钮节点。路径表达式“/”表示根节点。  
&nbsp;&nbsp;&nbsp;&nbsp;**更多说明:**  
&nbsp;&nbsp;&nbsp;&nbsp;使用绝对路径定位页面元素的好处在于可以验证页面是否发生变化。如果页面结构发生变化，可能会造成原先有效的XPath表达式失效。使用绝对路径定位是十分脆弱的，因为即使页面代码结构只发生微小的变化，也可能会造成原先有效的XPath定位表达式定位失效。因此，建议在自动化测试的定位实施环节中，优先考虑相对路径进行页面元素定位。  
### 2.2 使用相对路径来定位元素  
&nbsp;&nbsp;&nbsp;&nbsp;相对路径的每一步都根据当前节点集之中的节点来进行计算，起始于双正斜杠(//)。    
&nbsp;&nbsp;&nbsp;&nbsp;**目的：** 在被测网页中，查找第一个div标签下的“查询”按钮。  
&nbsp;&nbsp;&nbsp;&nbsp;**XPath定位表达式：** //input[@value="查询"]  
&nbsp;&nbsp;&nbsp;&nbsp;**Python定位语句：** query = driver.find_element_by_xpath('//input[@value="查询"]')  
&nbsp;&nbsp;&nbsp;&nbsp;**代码解释：**  
&nbsp;&nbsp;&nbsp;&nbsp;上述XPath定位表达式中的“//”表示从匹配选择的当前节点开始选择文档中的节点，而不考虑它们的位置。input[@value="查询"]表示定位value值为“查询”两个字的input页面元素。  
&nbsp;&nbsp;&nbsp;&nbsp;**更多说明:**  
&nbsp;&nbsp;&nbsp;&nbsp;相对路径的XPath定位表达式更加简洁，不管页面发生了何种变化，只要input标签的value属性值没变，始终可以定位到。推荐使用相对路径的XPath表达式，并且越简洁越好，可大大降低测试脚本定位的表达式的维护成本。    
### 2.3 使用索引号定位元素  
&nbsp;&nbsp;&nbsp;&nbsp;索引号表示某个被定位的页面元素在其父元素节点下的同名元素中的位置序号，需要从1开始。    
&nbsp;&nbsp;&nbsp;&nbsp;**目的：** 在被测网页中，查找第一个div标签下的“查询”按钮。  
&nbsp;&nbsp;&nbsp;&nbsp;**XPath定位表达式：** //input[2]  
&nbsp;&nbsp;&nbsp;&nbsp;**Python定位语句：** query = driver.find_element_by_xpath('//input[2]')  
&nbsp;&nbsp;&nbsp;&nbsp;**代码解释：**  
&nbsp;&nbsp;&nbsp;&nbsp;索引号定位方式是根据该页面元素在页面中相同标签名之间的出现的索引位置来进行定位的。上述XPath定位表达式表示查找页面中第二个出现的input元素，即测试页面上的“查询”按钮。  
&nbsp;&nbsp;&nbsp;&nbsp;**更多说明:**  
&nbsp;&nbsp;&nbsp;&nbsp;若在Firefox浏览器的FirePath插件使用“//input[1]”定位表达式进行页面元素定位，可以发现在被测试网页的HTML代码区域高亮显示了两行代码，两个div标签下的第一个input标签都被定位到，这和只查找第一个input元素相冲突，这是由于被测试网页中的两个div标签下都包含了input标签，XPath在查找的时候把每个div节点都当作相同的起始层级开始查找，所以用“//input[1]”表达式会同时查找到两个div节点下的第一个input元素。如果在两个div标签下还有嵌套的div，并且嵌套的div下也有input标签，使用“//input[1]”定位表达式，也会定位到嵌套div下的input标签，也就是说无论嵌套多少层HTML标签，只要这些HTML标签的子标签里有input标签，第一个input标签都会被定位点。因此在使用索引号定位页面元素的时候，需要注意网页HTML代码中是否包含了多个层级完全相同的代码结构，若出现了这种情况，就需要修改定位表达式，以确保自动化测试脚本中使用的定位表达式能唯一定位所需要的页面元素。  
&nbsp;&nbsp;&nbsp;&nbsp;但如果想同时定位多个相同的input页面元素，可以使用如下Python语句：  
&nbsp;&nbsp;&nbsp;&nbsp;inputList = driver.find_elements_by_xpath("//input[1]")  
&nbsp;&nbsp;&nbsp;&nbsp;将定位的多个元素存储到list对象中，然后根据list对象的索引号获取想要的页面元素。但如果发现页面元素会经常增加或减少，就不建议使用索引号定位的方式，因为页面变化很可能会让使用索引号的XPath定位表达式失效。  
&nbsp;&nbsp;&nbsp;&nbsp;基于实例中的被测试网页，下面给出更多通过索引号定位的实例。  

预期定位的页面元素|定位表达式实例|使用的属性值  
-----|-----|-----  
定位第二个div下的超链接|//div[last()]/a|div[last()]表示最后一个div元素，last()函数获取的是指定元素的的最后索引号  
定位第一个div中的超链接|//div[last()-1]/a|div[last()-1]表示倒数第二个div元素  
定位最前面一个属于div元素的子元素中的input元素|//div/input[position()<2]|position()函数获取当前元素input的位置序列号  

### 2.4 使用页面元素的属性值定位元素  
&nbsp;&nbsp;&nbsp;&nbsp;在定位页面元素的时候，经常会遇到各种复杂结构的被测试网页，并且很多页面元素也没有设计ID，Name等属性，同时又不想使用绝对路径或索引号来定位页面元素，但是发现要被定位的页面元素拥有某些固定不变的的属性和属性值，此时推荐使用属性定位方式来定位页面元素。  
&nbsp;&nbsp;&nbsp;&nbsp;**目的：** 在被测网页中，定位网页中的第一张img元素。  
&nbsp;&nbsp;&nbsp;&nbsp;**XPath定位表达式：** //img[@alt="div1-img1"]  
&nbsp;&nbsp;&nbsp;&nbsp;**Python定位语句：** img = driver.find_element_by_xpath('//img[@alt="div1-img1"]')  
&nbsp;&nbsp;&nbsp;&nbsp;**代码解释：**  
&nbsp;&nbsp;&nbsp;&nbsp;表达式使用了相对路径再结合元素拥有的特定属性的方法进行定位，定位元素img的属性是"alt",其属性值为"div1-img1"，使用@符号指明后面接的是属性，并同属性及属性值一同写到元素后面的方括号中。  
&nbsp;&nbsp;&nbsp;&nbsp;**更多说明:**  
&nbsp;&nbsp;&nbsp;&nbsp;被测试网页的元素通常会包含各种各样的属性值，并且很多属性值是唯一的。若能确认属性值不变且唯一，建议使用相对路径再结合属性值的定位方式来编写XPath定位表达式，使用此方法可以解决99%的页面元素定位难题。  

预期定位的页面元素|定位表达式实例|使用的属性值  
-----|-----|-----  
定位页面的第一张图片|//img[@href='http://www.sougou.com']|使用img标签的href属性值  
定位第二个div中的第一个input输入框|//div[@name='div2']/input[@name='div2input']|使用div标签的name属性值，使用input标签的name属性值  
定位第一个div中的第一个链接|//div[@id='div1']/a[@href='http://www.sougou.com']|使用div标签的ID属性值，使用a标签的href属性值  
定位页面的查询按钮|//input[@type='button']|使用type属性值  

### 2.5 使用模糊属性值定位元素  
&nbsp;&nbsp;&nbsp;&nbsp;模糊属性值定位方式表示使用属性值的一部分内容进行定位。在自动化测试的实施过程中，常常会遇到页面元素的属性值是动态生成的，也就是时候每次访问属性值都不一样，此类页面元素会加大定位难度，使用模糊属性值定位方式可以解决一部分此类难题，但前提是属性值中有一部分内容保存不变。XPath提供了一些课实现模糊属性值定位的函数。  

XPath函数|定位表达式实例|使用的属性值  
-----|-----|-----  
start-with(str1,str2)|//img[start-with(@alt,'div1')]|查找属性alt的属性值以“div1”关键字开始的页面元素  
contains(str1,str2)|//img[contains(@alt,'img')]|查找alt属性的属性值包含“img”关键字的页面元素，只要包含即可，无需考虑位置  

&nbsp;&nbsp;&nbsp;&nbsp;contains()函数属于XPath的高级用法，使用场景较多，尽管页面元素的属性值经常发生变化，但只要其属性值有几个固定不变的关键字，就可以使用contains()函数进行定位。  

### 2.6 使用XPath轴(Axes)定位元素  
&nbsp;&nbsp;&nbsp;&nbsp;轴可以定义相对于当前节点的节点集。使用XPath(Axes)定位方式可根据在文档树中的元素相对位置关系进行页面元素定位。先找到一个相对好定位的元素，让它作为轴，根据它和要定位元素间的相对位置关系进行定位，可以解决一些元素难定位的问题。  

XPath轴关键字/轴的含义说明|定位表达式实例|使用的属性值  
-----|-----|-----  
parent:选择当前节点的上层父节点|//img[@alt='div2-img2']/parent::div|查找属性alt的属性值为div2-img2的img元素，并基于该img元素的位置找到它上一级的div页面元素  
child:选择当前节点的下层所有子节点|//div[@id='div1']/child::img|查找到ID属性为div1的div元素，并基于div的位置找到它的下层节点中的img元素  
ancestor:选择当前节点的所有上层的节点|//img[@alt='div2-img2']/ancestor::div|查找到属性alt的属性值为div2-img2的img元素，并基于该img元素的位置找到它上级的div页面元素  
descendant:选择当前节点所有下层的节点(子，孙等)|//div[@name='div2']/descendant::img|查找到属性name的属性值为div2-img2的img元素，并基于该元素的位置查找到它的下级所有节点中的img页面元素    
following:选择在当前节点之后显示的所有节点|//div[@id='div1']/following::img|查到到ID属性值为div1的div页面元素，并基于div的位置找到它后面节点中的img页面元素     
following-sibling:选择当前节点后续所有兄弟节点|//a[@href='http://www.sougou.com']/following-sibling::input|查找到链接地址为http://www.sougou.com的链接页面元素a,并基于链接的位置找到它后续的兄弟节点中的input页面元素      
preceding:选择当前节点前面的所有节点|//img[@alt='div2-img2']/preceding::div|查到到属性alt的属性值为div2-img2的图片页面元素img，并基于图片的位置找到它前面节点中的div页面元素    
preceding-sibling:选择当前节点前面的所有兄弟节点|//input[@value='查询']/preceding-sibling::a[1]|查找到value属性值为“查询”的输入框页面元素，并基于该输入框的位置找到它前面同级节点中的第一个链接页面元素      

&nbsp;&nbsp;&nbsp;&nbsp;**更多说明:**  
&nbsp;&nbsp;&nbsp;&nbsp;有时候我们会在轴的后面加一个星号(*)，表示通配符，比如//input[@value='查询']/preceding-sibling::*,它表示查找属性value的值为“查询”的输入框input元素前面所有的同级元素，但不包括input元素本身。


