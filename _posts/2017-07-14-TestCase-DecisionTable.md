---
layout: post
category : 测试
title: 用例设计之判定表
tagline: Test Design
tags: Test
---

# 1. 判定表的介绍  
在对软件进行需求分析时, 市场部人员需要跟用户进行不断的沟通, 这时可能会根据软件功能的期望让用户填一些调查表格, 用户会根据条件选择自己期望达到的效果。如果将条件称为输入, 将期望效果称为输出, 这就非常接近于软件测试中的测试用例。如果由于条件的不同组合会得到不同的一些输出, 那么这样的问题就适合使用**判定表**来进行测试用例的设计。  

## 1.1 判定表通常由四个部分组成  
	(1)、条件桩(Condition Stub): 列出了问题的所有条件。除特别说明, 认为列出的条件的次序无关紧要。  
	(2)、动作桩(Action Stub) : 根据条件的组合可能导致的动作。一般排列顺序没有约束。  
	(3)、条件项(Condition Entry) : 由条件桩列出条件的可能取值, 即条件的真和假。  
	(4)、动作项(Action Entry) : 列出在不同条件排列组合下可能采取的操作。   

## 1.2 规则及规则合并
	(1)、规则: 由不同的条件导致不同的动作就称为规则, 一般体现在判定表中就是不同的输入得到不同的输出。在判定表中贯穿条件项和动作项的一列就是一条规则。
	(2)、化简: 因为初始化判定表包括条件的所有组合, 这时有些组合可能是不能实现的, 有些动作可能是由一些相似的条件组成的, 这时就需要按照等价类划分的原则进行化简。

# 2. 普通示例  

## 2.1 判定表建立步骤  

	1、分析功能说明书, 确定条件、规则的个数。
	2、根据分析, 列出所有的条件桩和动作桩。
	3、根据分析输入填入条件项。  
	4、根据输出填入动作项, 根据排列组合得到初始决策表。
	5、简化,合并相似规则(相同动作)。

## 2.2 判定表举例  

“对功率大于50马力的机器、维修记录不全或已运行10年以上的机器，应给予优先的维修处理……” 。这里假定，“维修记录不全”和“优先维修处理”均已在别处有更严格的定义；请建立判定表。   
  
**步骤：**   
&nbsp;&nbsp;&nbsp;&nbsp;1.确定规则的个数：这里有3个条件，每个条件有两个取值，故应有2*2*2=8种规则。   

&nbsp;&nbsp;&nbsp;&nbsp;2.列出所有的条件桩和动作桩：  

![pic1](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/1-2017-07-14-TestCase-DecisionTable.png)   

&nbsp;&nbsp;&nbsp;&nbsp;3.填入条件项。  

![pic2](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2-2017-07-14-TestCase-DecisionTable.png)  

&nbsp;&nbsp;&nbsp;&nbsp;4.填入动作桩和动作顶。这样便得到形如图的初始判定表。  

![pic3](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/3-2017-07-14-TestCase-DecisionTable.png)  

&nbsp;&nbsp;&nbsp;&nbsp;5.化简。合并相似规则后得到图。  

![pic4](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/4-2017-07-14-TestCase-DecisionTable.png)  

&nbsp;&nbsp;&nbsp;&nbsp;6.然后根据最后化简的结果，生成相应的测试用例。  

# 3.判定表的优缺点
**优点：**把复杂的问题按各种可能的情况一一列举，简明而易于理解，也避免遗漏。  

**缺点：**    

- 不能表达重复执行的动作，如循环结构。  

- 判定表不能很好的伸缩。如有n个条件的判定表有2n个规则。  

# 4.适合使用判定表设计测试用例的条件：
1.  规格说明以判定表形式给出，或很容易转换成判定表的。
2.  条件的排列顺序不会也不影响执行哪些操作。
3.  规则的排列顺序不会也不影响执行哪些操作。
4.  每当某一规则的条件已近满足，并确定要执行的操作后，不必检验别的规则。
5.  如果某一规则得到满足要执行多个操作，这些操作的执行顺序无关紧要。