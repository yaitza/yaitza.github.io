---
layout: post
category : 测试
title: 测试分析
tagline: Test Design
tags: Test
---

# 1.需求分析概念  

&nbsp;&nbsp;&nbsp;&nbsp;在系统工程及软件工程中，需求分析指的是在创建一个新的或改变一个现存的系统或产品时，确定新系统的目的、范围、定义和功能时所要做的所有工作。需求分析是软件工程中的一个关键过程。在这个过程中，系统分析员和软件工程师确定顾客的需要。只有在确定了这些需要后他们才能够分析和寻求新系统的解决方法。
&nbsp;&nbsp;&nbsp;&nbsp;需求分析的质量，考虑是否周全将直接关系到后续工作的开展。

# 2. 需求分析视角  

## 2.1 视角的分类

&nbsp;&nbsp;&nbsp;&nbsp;作为一名测试人员，我们接触到更多的是产品经理的需求文档，每当我们从产品经理手中接过需求文档时，往往心中的第一个疑问就是怎么“看”，在解答这个问题之前，我们先来了解下关于“需求三个视角”的描述：

![pic1](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/1-2017-07-18-TestAnalysis-Model.png) 

&nbsp;&nbsp;&nbsp;&nbsp;将需求分为三个视角解读：数据视角、功能视角和行为视角，并分别列举了类图，活动图和状态图，同时还特别强调了三种视角并非独立存在而是具有一定融合、交叉的关系。

	数据视图：描述输入输出数据结构、静态结构及依赖关系(也称为结构视图)。  
	功能视图：描述系统实现的功能，输入输出数据的处理。  
	行为视图：描述系统如何运行和具体功能实现。  

&nbsp;&nbsp;&nbsp;&nbsp;因此，当接到一个业务需求、产品特性或是整个系统时，我们可以通过这三个视角来解读被测系统。那么，每一种视角究竟有哪些工具可以让我们进行选择使用呢？

	从结构上来说，有类图[1]，E-R图[2]，组件图[3]；  
	从功能上来说，有用例图[4]，因果图[5]，决策表[6]；  
	从行为上来说，有活动图[7]，状态图[8]，序列图[9]。  

&nbsp;&nbsp;&nbsp;&nbsp;特定的视角惯以常用的分析图，为了达到更加便捷，清晰地阐明需求，使得需求覆盖更加完全。

## 2.2 视角的选择

&nbsp;&nbsp;&nbsp;&nbsp;选择合适的切入口对产品需求进行快速有效地分析。

### 2.2.1 根据需求特点选择

&nbsp;&nbsp;&nbsp;&nbsp;“名词”较多的优先选择结构视图。
&nbsp;&nbsp;&nbsp;&nbsp;“功能、逻辑、算法”较多的优先选择功能视图。

### 2.2.2 根据项目阶段来判断

&nbsp;&nbsp;&nbsp;&nbsp;需求评审阶段：这个阶段的目标是消除歧义、达成一致，测试建模的主要功能也是促进理解和交流，因此，这个阶段我们使用各类视图建模时不易过度关注细节，大体描述主要流程即可，我们可以随着项目的开展对模型进行迭代更新。
&nbsp;&nbsp;&nbsp;&nbsp;产品实现阶段：产品实现过程中，同时关注各种视图中的细节更有利于理解和评审真实实现逻辑，修正测试模型，生成高质量测试方案。根据产品设计构建的模型还可以用来检验设计的正确性合理性。但是要注意避免通过代码实现来推到测试模型。
&nbsp;&nbsp;&nbsp;&nbsp;产品提测阶段：提测时主要是对照已有模型（如果前期有建模准备）进行更新补充；或者直接应用探索式测试相关的启发式边建模边测试边反馈修正。
### 2.2.3 根据个人喜好

&nbsp;&nbsp;&nbsp;&nbsp;“当你无从下手的时候，就从活动图开始”。其实道理很简单，活动图（流程图）似乎与生俱来一直都很熟悉。熟悉的视角肯定能让我们更快的入手，但更重要的一点是一定不能忽视从其他的视角去做一些补充。
&nbsp;&nbsp;&nbsp;&nbsp;通过以上视角对需求的理解与分析，将测试思路与内容转化为条理清晰，覆盖全面的测试用例是我们的诉求。
## 2.3 测试模型

&nbsp;&nbsp;&nbsp;&nbsp;对于上文提到不同视角对需求进行分析提炼，最终生成对于我们有用的测试用例；将这个过程方法论就是我们的测试模型。

### 2.3.1 测试模型的概念
&nbsp;&nbsp;&nbsp;&nbsp;测试建模，将测试思路或内容形成条理清晰、系统全面的模型而展开的测试（MBT）。

	SUT：System Under Test，被测系统模型，也称类比模型。
	TRM：Test Ready Model，测试准备模型，也称分析模型。

#### 2.3.1.1 SUT模型
&nbsp;&nbsp;&nbsp;&nbsp;SUT模型是心智模型（对产品的理解、设想和体验）的外化（以及与现有模型的整合），是一种图形化或形式化的类比模型。它涉及到不同的层次（如系统、组件和工作环境）、不同视角（如语境/上下文、组件与结构、功能、行为和用户体验）和不同关注点（如数据类型、因果关联、程序结构、任务控制、动作、事件和接口）等。
 
![pic2](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2-2017-07-18-TestAnalysis-Model.png) 

&nbsp;&nbsp;&nbsp;&nbsp;经过抽象、泛化和删减后，SUT模型只保留有助于实现特定测试目的的特征。在生成可执行测试用例前，SUT模型的实例化可能用到的技术（如Figure 2）包括Finite State Machine (FSM)，Message Sequence Chart (MSC)， Control Flow Graph (CFG)，Event Flow Diagram，MarkovChains和UML Testing Profile 等。

#### 2.3.1.2 TRM模型
&nbsp;&nbsp;&nbsp;&nbsp;TRM模型是对SUT模型的扩展和转化，以使模型达到可测试的标准；该模型也可独立使用，即给出相关信息，我们就可以设计或使用一套测试设计算法，用来产生可以运行的测试用例。它根据SUT模型特征和项目实际情况增加或凸显质量风险信息。必要时TRM需要创建新的模型，这是测试建模的主要难点之一，但也体现了我们价值所在。另外，它转化SUT模型以达到可测试标准，并增加“怎么测”的信息，同时为SUT模型修改重构提供反馈。

#### 2.3.1.3 SUT与TRM区别

&nbsp;&nbsp;&nbsp;&nbsp;SUT和TRM模型有密切的关系，那么它们的侧重点有哪些区别呢？相对来说，SUT层次更高，更温和，以描述被测对象为己任（更抽象）；而TRM更接地气，更直接，以揭露风险为使命（更具体）。实践中，TRM模型一般以发现SUT的潜在风险为导向。与SUT建模相比，TRM缺少现成的系统的方法论指导，缺少可参考借鉴的方法，更倚重经验，却缺少经验积累。

SUT|TRM   
-----|-----    
描绘了测试对象|体现测试策略，如测什么，测多少及怎么测    
系统思维|创造性思维   
“面”(等价类，平均值)|“点”(边界值)   
构建为目的，重点关注可能产生质量风险的地方|直接为“破坏”，“攻击”服务，关注如何揭露质量问题   

### 2.3.2 测试建模的方法
&nbsp;&nbsp;&nbsp;&nbsp;测试建模的方法从宏观上来看，主要分为SUT建模和TRM建模。从微观上来看又派生了很多的模型。在实际工作中，我们拿到被测系统后，会在脑海里进行瞬时画像建模，也就是构建了心智模型。而从心智模型过渡到测试用例，中间过程的不同决定了不同的测试设计，如下图所示。

![pic3](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/3-2017-07-18-TestAnalysis-Model.png) 

	路径一[红色箭头]：从心智模型（Mental Model）直接得到测试用例（Ad-hoc Test Design，基于临时需求）；
	路径二[黄色箭头]：从心智模型得到TRM模型，再由TRM模型生成测试用例（TraditionalExploratory传统测试设计）；
	路径三[蓝色箭头&紫色箭头]：从心智模型到SUT模型，再由SUT模型生成测试用例（教科书式）；
	路径四[蓝色箭头]：从心智模型到SUT模型，再由SUT模型到TRM模型，最终由TRM模型生成测试用例（MBT）。

&nbsp;&nbsp;&nbsp;&nbsp;由上图所示，MBT强调中间过程。特别说明的是，MBT是一个循序渐进、逐步完善的过程，需要将被测系统的各个方面进行考虑，在发布周期之前形成完善的模型有利于整个产品的开发、测试和发布等工作，如下图所示。
 
![pic4](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/4-2017-07-18-TestAnalysis-Model.png)

&nbsp;&nbsp;&nbsp;&nbsp;我们拿到被测需求后，首先会进行SUT抽象建模；分析需求进行TRM建模；初步模型验证；基于模型可控地生成测试用例；优化并生成可执行测试用例。根据用户关注重点的差异，第一步可以对被测系统进行功能建模，也可以进行用户使用环境建模；第四步可以针对一些模式（Patterns）或测试特异性（Test specifications）来生成用例，也可以根据测试覆盖率等软件测试度量规则（Criteria）来生成测试用例。

### 2.3.3 怎样展开测试建模
&nbsp;&nbsp;&nbsp;&nbsp;在实际测试过程中，我们拿到的输入通常是需求说明书或是开发的实现代码等，经过测试人员的建模加工后，最终生成测试用例。针对需求分析从整体角度，我们往往会使用相关模型分析深入理解被测需求（包括识别关键变量），基于分析结果再进行具体的模型构建，如业务流程图、决策树等等；接着，结合关键变量进行风险分析，完善模型；最终将模型通过自动化或手工方式转换成用例。
 
![pic5](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/5-2017-07-18-TestAnalysis-Model.png)

### 2.3.4 实例解析

**需求描述**

![pic6](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/6-2017-07-18-TestAnalysis-Model.png) 

**需求理解**

&nbsp;&nbsp;&nbsp;&nbsp;SUT建模
 
![pic7](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/7-2017-07-18-TestAnalysis-Model.png) 

&nbsp;&nbsp;&nbsp;&nbsp;SUT转化为TRM模型
 
![pic8](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/8-2017-07-18-TestAnalysis-Model.png) 

&nbsp;&nbsp;&nbsp;&nbsp;根据TRM各个节点测判断依据生成对应的测试用例。

# 参考文献

	[1]类图：http://www.uml.org.cn/oobject/201211231.asp
	[2]E-R图：https://zh.wikipedia.org/wiki/ER%E6%A8%A1%E5%9E%8B
	[3]组件图：http://blog.csdn.net/fanxiaobin577328725/article/details/51647248
	[4]因果图：http://blog.csdn.net/vincetest/article/details/1475414
	[5]活动图：http://www.cnblogs.com/ywqu/archive/2009/12/14/1624082.html
	[6]决策表：http://www.51testing.com/html/70/n-3578470.html
	[7]状态图：http://www.cnblogs.com/ywqu/archive/2009/12/17/1626043.html
	[8]用例图：https://yaitza.github.io/2017-05-13-UML-Case
	[9]序列图：http://blog.csdn.net/tianhai110/article/details/6361338
