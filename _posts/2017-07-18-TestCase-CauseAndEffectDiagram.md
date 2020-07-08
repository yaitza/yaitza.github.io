---
layout: post
category : 测试
title: 用例设计之因果图
tagline: Test Design
tags: Test
---

# 1. 因果图的介绍  
&nbsp;&nbsp;&nbsp;&nbsp;因果图法是一种利用图解法分析输入的各种组合情况，从而设计测试用例的方法，它适合于检查程序输入条件的各种组合情况。  

## 1.1 主要的因果关系  

&nbsp;&nbsp;&nbsp;&nbsp;因果图中主要有两种节点：原因节点与结果节点，这两种节点分别有两种状态：0状态，1状态。若原因为假，则为0状态，否则为1状态；若结果发生，则为1状态，否则为0状态。通常情况下，原因节点用符号ci(i≥1)表示，结果节点用符号ei(i≥1)表示。

&nbsp;&nbsp;&nbsp;&nbsp;在因果图中，原因与结果的主要关系有：与，或，非，恒等。
  
	(1)恒等关系表示原因与结果的一对一关系。若原因为真，则结果就为真；若原因为假，则结果就为假。  
	(2)非关系表示原因与结果间的否定，若原因为真，结果就为假；若原因为假，结果就为真。  
	(3)或关系表示只要有一个原因为真，结果就为真；当且仅当所有的原因为假，结果才为假。  
	(4)与关系表示当且仅当所有原因都为真，结果才为真；只要有一个原因为假，结果就为假。 
  
![pic1](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/1-2017-07-18-TestCase-CauseAndEffectDiagram.png)   

## 1.2 原因之间的约束

&nbsp;&nbsp;&nbsp;&nbsp;原因之间的约束是指输入条件之间存在的依赖关系(如某些输入条件本身不可能同时出现，或者必须同时出现等)。例如某企业的库存控制管理系统，其功能是跟踪企业产品的库存变化情况。对于每种产品库存的3种状态：库存正常，库存不足，库存为空，在任何情况下这3种状态最多只能有一个为真。

&nbsp;&nbsp;&nbsp;&nbsp;在因果图种原因之间的约束主要有排异约束，包容约束，唯一约束，要求约束。

	(1)互斥约束，表示在任何情况下，各个原因之间只能有一个为真（不能同时为真），但可以同时为假，在因果图种用符号E表示。
	(2)包含约束，存在包容约束的各个原因中总有一个为真（可以同时为真），但不可以同时为假，在因果图种用符号I表示。
	(3)唯一性约束表示各个原因中有且仅有一个为真，在因果图种用符号O表示。
	(4)要求约束又称必要性约束，若原因c1，c2之间存在要求约束，表示若c1为真，则c2也必须为真，在因果图中用符号R表示。

![pic2](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2-2017-07-18-TestCase-CauseAndEffectDiagram.png) 

# 2. 普通示例  

&nbsp;&nbsp;&nbsp;&nbsp;有一个处理单价为1元5角钱的盒装饮料的自动售货机软件。若投入1元5角硬币，按下“可乐”、“雪碧”、或“红茶”按钮，相应的饮料就送出来。若投入的是2元硬币，在送出饮料的同时退还5角硬币。
  
**步骤：**   

&nbsp;&nbsp;&nbsp;&nbsp;1.确定需求中的原因与结果   

![pic3](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/3-2017-07-18-TestCase-CauseAndEffectDiagram.png) 

&nbsp;&nbsp;&nbsp;&nbsp;2.确定原因与结果的逻辑关系   

&nbsp;&nbsp;&nbsp;&nbsp;C1与C2需要一个中间结果Cm1(已投币)，C3、C4、C5需要一个中间结果Cm2(已按钮)。 

&nbsp;&nbsp;&nbsp;&nbsp;3.确定因果图中的约束  

&nbsp;&nbsp;&nbsp;&nbsp;C1与C2是或的关系，C3、C4、C5是或的关系。 

&nbsp;&nbsp;&nbsp;&nbsp;4.画出因果图  

![pic4](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/4-2017-07-18-TestCase-CauseAndEffectDiagram.png)  

&nbsp;&nbsp;&nbsp;&nbsp;5.根据因果图并得到决策表  

![pic5](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/5-2017-07-18-TestCase-CauseAndEffectDiagram.png)  

&nbsp;&nbsp;&nbsp;&nbsp;6.化简决策表  

![pic6](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/6-2017-07-18-TestCase-CauseAndEffectDiagram.png)  

&nbsp;&nbsp;&nbsp;&nbsp;6.根据决策表生成测试用例

![pic7](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/7-2017-07-18-TestCase-CauseAndEffectDiagram.png) 