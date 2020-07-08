---
layout: post
category: 测试
title: Android电量测试方法研究与总结
tagline: Android
tags: Android
---

# 1. 研究背景

在2017年Google I/O大会上，Google发布了Google Play管理中心的新功能：Android vitals。当app在大量设备上运行时，Android vitals会收集与应用性能相关的各种匿名数据，比如：与app稳定性相关的数据、app启动时间、电量使用情况、渲染时间以及权限遭拒等等，这些数据会被分析整理后展示在Google Play管理中心的Android vitals dashboard中。Android vitals 中需要开发者重点关注的核心指标有：crash率、ANR率、excessive wakeups（过渡唤醒）、stuck wake locks（唤醒锁定卡住）。其他指标，需根据应用类型选择性关注（Android vitals中的指标总览见图1-1）。若app某些指标表现很差，会影响用户体验，并且会导致应用在Google Play商店中的等级很低、排名靠后（APP指标异常示例图见图1-2）。开发者可以通过分析Android vitals中提供的一些参照指标，采取相应的措施来优化app。  
![Android vitals平台检测指标总览](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2019-02-20-Android-Quantity-1.jpg)   

  图1-1 Android vitals平台检测指标总览
  
![某APP指标异常示例图](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2019-02-20-Android-Quantity-2.jpg)   

  图1-2 某APP指标异常示例图


# 2. 核心指标详细信息

要对APP的指标进行监控，首先要明确该指标在Android vitals中是如何进行统计的，这一节主要介绍电量相关核心指标的基本概念和计算方式。

## 2.1 Stuck partial wake locks（部分唤醒锁定卡住）
- A.WakeLock（唤醒锁）基本概念：

Android系统本身为了优化电量的使用，会在没有操作时进入休眠状态, 来节省电量。为了便于开发(很多应用不可避免的希望在灭屏后还能运行一些事儿，或是要保持屏幕一直亮着--比如播放视频)，Android提供了一个PowerManager.WakeLock的东西。我们可以用WakeLock来保持CPU运行，或是防止屏幕变暗/关闭，让手机可以在用户不操作时依然可以做一些事儿。然而，获取WakeLock很容易，释放不好就会成为难题，消耗电量。  

例如我们获取了一个WakeLock来保持CPU运转，做一个复杂运算并将数据上传到后台服务器, 然后释放该WakeLock。然而这个过程可能并不像我们想象的那么快，可能因为比如服务器挂掉，计算出了异常等等WakeLock没有释放。问题就来了，CPU会一直得不到休眠，而大大增加耗电。

唤醒锁可划分为并识别四种用户唤醒锁：

![四种用户唤醒锁](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2019-02-20-Android-Quantity-3.jpg) 

自 API 等级 17 开始，FULL_WAKE_LOCK 将被弃用。应用应使用FLAG_KEEP_SCREEN_ON。

相关链接： <https://developer.android.com/reference/android/os/PowerManager>

- B.Partial wake locks（部分唤醒锁）：

部分唤醒锁可确保CPU正常运行，但屏幕和键盘背光可以关闭。如果运行在后台的APP长时间持有某个部分唤醒锁，就导致部分唤醒锁卡住。这种情况十分消耗设备电量，因为它会阻止设备进入低电量状态。Android vitals重点关注了stuck partial wake locks这项指标，当你的APP存在唤醒锁定卡住的现象时，它会通过Play管理中心给出告警（APP出现部分唤醒锁定卡住示例图见图2-1），并从各个维度给出相关的详细统计图（如图2-2中给出每个工作时段后台wake lock最长持续时间分布图）。当出现以下情况时，Android vitals会报告唤醒锁定卡住：

	* 至少70%以上的battery sessions发生过至少一次、长达一小时以上的部分唤醒锁定。  
	* 当只在后台运行时，至少10%以上的battery sessions发生过至少一次、长达一小时以上的部分唤醒锁定。

（ps：battery session指两次电池充满电之间的时间间隔，Android vitals展示的battery sessions是所有app测试用户的battery session合计。）

相关链接： <https://developer.android.google.cn/topic/performance/vitals/wakelock> 

![某APP出现部分唤醒锁定卡住](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2019-02-20-Android-Quantity-4.jpg) 

图2-1 某APP出现部分唤醒锁定卡住（后台）示例图  

![每个工作时段后台wake lock最长持续时间的分布图](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2019-02-20-Android-Quantity-5.jpg) 

图2-2 每个工作时段后台wake lock最长持续时间的分布图

## 2.2 Excessive wakeups（过渡唤醒）
- A.Wakeups 基本概念  

Wakeups 是AlarmManager API中的一种机制，开发者可以设置一个alarm在特定的时间来唤醒设备。当某个唤醒alarm触发，设备会走出低电量模式，在执行alarm的onRecieve（）或onAlarm（）方法的时候，Alarm Manager会持有一个部分唤醒锁。如果wake alarms频繁触发，会耗尽设备电量。Android vitals中展示了app的过渡唤醒次数。

Alarm有以下四种类型：

	* RTC_WAKEUP   
	
在指定的时刻（设置Alarm的时候），唤醒设备来触发Intent。  
	* RTC   
	
在一个显式的时间触发Intent，但不唤醒设备。 
  
	* ELAPSED_REALTIME   
	
从设备启动后，如果流逝的时间达到总时间，那么触发Intent，但不唤醒设备。流逝的时间包括设备睡眠的任何时间。注意一点的是，时间流逝的计算点是自从它最后一次启动算起。  

	* ELAPSED_REALTIME_WAKEUP  
	
从设备启动后，达到流逝的总时间后，如果需要将唤醒设备并触发Intent。
在Android vitals中只列出了RTC_WAKEUP和ELAPSED_REALTIME_WAKEUP两种类型的唤醒数据，Google会统计每小时发生10次以上wakeup的电池工作时段百分比（APP发生过渡唤醒示例见图2-3）。分别从应用版本、wakeup标记、设备、Android版本等几个维度统计每小时的Alarm Manager wakeup次数（每个工作时段中每小时的wackup分布图见图2-4）

![某APP发生过渡唤醒示例图](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2019-02-20-Android-Quantity-6.jpg) 

图2-3 某APP发生过渡唤醒示例图

![每个工作时段每小时wakeup次数分布图](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2019-02-20-Android-Quantity-7.jpg) 

图2-4 每个工作时段每小时wakeup次数分布图

# 3、测试方法研究  
## 3.1 传统电量测试方法回顾  

我们之前也对腾讯视频主线版本进行过电量测试，之前关注的重点在于APP在各场景中耗电量是否正常，是从比较宏观的角度去进行测试的，采取的测试方法主要是物理仪器测试法和GT测试法。

- A.物理仪器测试法（电流表等）    

在保持电压恒定的情况下，获取各场景平均电流值来统计系统耗电情况，通过此方法可以从大体上看出APP电量消耗是否正常，若仪器精度大，此方法测出的电量值是最准确的。
缺陷：此方法只能测试整个手机的电流，不能区分APP，受影响的因素多，如屏幕亮度大小、音量大小等等，要保证每次测试的环境完全一致是不可能的。  

![物理仪器测电量](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2019-02-20-Android-Quantity-8.jpg) 

图3-1 物理仪器测电量

- B.GT测试法 

GT（随身调）是由MIG专项测试组自主研发的APP随身调测平台，它是直接运行在手机上的“集成调测环境”(IDTE, Integrated Debug Environment)。利用GT，仅凭一部手机，无需连接电脑，您即可对APP进行快速的性能测试(CPU、内存、流量、电量、帧率/流畅度等等)、开发日志的查看、Crash日志查看、网络数据包的抓取、APP内部参数的调试、真机代码耗时统计等。   
通过GT，可以采集手机耗电量相关数据：电流、电压、电量、温度等，通过分析这些数据，可以对整个手机的电量使用情况进行分析。

![物理仪器测电量](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2019-02-20-Android-Quantity-9.jpg)  

缺点：和物理仪器测试方法一样，采用GT测试也只能获取到整个手机的电量数据，无法只关注单独APP，且受各种因素影响较大。

## 3.2 国际版电量测试方法预研

由于国际版APP在Google Play上发布，我们做电量测试不仅仅需要关注整个APP的电量使用情况是否正常，还需要关注APP持有 wack lock和使用alarm的情况。因此，传统的电量测试方法已经无法满足我们的需求，我们需要在此基础上增加额外的测试方法。

- A.Batterystats/ bugreport

Android5.0后，电量数据可通过dumpsys batterystats获取。Android系统统计耗电量的基本公式是W=U*I*t。在手机中，U一般恒定不变，因此可以单独通过Q（电容量）=I*t来表示电量。核心类BatterStatsImpl提供App各部件运行时间、PowerProfile提供部件电流数值。Android部件电流信息存于：power_profile.xml文件中，每个OEM厂商都有私有的power_profile.xml文件，PowerProfile通过读取该文件获取访问部件电流数值（图3-3是samsung某型号的power_profile.xml）。Android系统以uid为单位，依次统计每个apk的使用cpu使用耗电量、wake lock耗电量、移动数据耗电量、wifi数据耗电量、wifi维持耗电量、wifi扫描耗电量、各传感器耗电量。其中wake lock消耗的电量只统计了持有Partial wake lock的耗电量，正好是我们需要关注的唤醒类型，因此我们可以通过分析batterystats获得的电量数据来测试app持有Partial wake lock情况。
Android为了方便开发人员分析整个系统平台和某个app在运行一段时间之内的所有信息，专门开发了bugreport工具。bugreport文件中记录了系统允许过程中的各种log信息，其中也包括了耗电量信息。通过分析bugreport中的电量相关数据也能获取APP持有Partial wake lock的信息。
ps：Uid与App关系：2个App签名和sharedUserId相同，则在运行时，他们拥有相同Uid。就是说processAppUsage统计的可能是多个App的耗电量数据，对于普通App，出现这种情况的几率较少，而对于Android系统应用则较为常见。

![wack lock耗电量计算源代码](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2019-02-20-Android-Quantity-10.jpg) 

图3-2 wack lock耗电量计算源代码

![sumsung某型号power_profile.xml](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2019-02-20-Android-Quantity-11.jpg) 

图3-3 sumsung某型号power_profile.xml

数据准备：

先断开adb服务，然后开启adb服务。

- (1)adb kill-server   
- (2)adb start-server   

由于开发时做电量记录时会打开很多可能造成冲突的东西，为了保险起见，重启adb命令。

重置电池数据、收集数据

- (3) adb shell dumpsys batterystats  --enable full-wake-history   
- (4) adb shell dumpsys batterystats --reset   
- (5) adb shell logcat -c  

通过以上命令来打开电池数据的获取以及重置，清除干扰的数据，清除历史日志。  

获取电量报告

把数据线拔掉，防止数据线造成充放电数据干扰。然后做一些测试的case，经过一段时间后，重新连接手机确认adb连上了，运行以下命令来将bugreport的信息保存到txt文件中。

- (6) adb bugreport >D:/bugreport.txt   

或者用下面的命令也可以，官网上记述的内容，经实践，无法被读取…

- (7) adb shell dumpsys batterystats > batterystats.txt
- (8) adb shell dumpsys batterystats > com.example.app(包名) >batterystats.txt

ps：在此注意一定要等到该条命令执行完(稍微会有些慢)后，再打开bugreport.txt文件，之前遇到过没有导出完，就点开，信息缺失的情况，导致无法成功生成图表。

- B.battery historian

生成的bugreport文件有的时候异常庞大，能够达到15M+，想一想对于一个txt文本格式的文件内容长度达到了15M+是一个什么概念，如果使用文本工具打开查看将是一个噩梦。因此google针对android 5.0（api 21）以上的系统开发了一个叫做battery historian的分析工具，这个工具就是用来解析这个txt文本文件，然后使用web图形的形式展现出来，这样出来的效果更加人性化，更加可读。我们可以使用该工具对bugreport文件进行解析，更轻松的获取电量相关数据。
battery historian的安装可以参考以下链接：   

<https://github.com/google/battery-historian>     

<https://developer.android.com/studio/profile/battery-historian>    

也可以直接使用在线版本： 
<https://bathist.ef.lc/>

数据分析：

* 选择腾讯视频app  

![选择腾讯视频app](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2019-02-20-Android-Quantity-12.jpg) 

* Wacklocks表格中展示app持有的wacklock，持有时间及数量，通过这个表格我们可以看到我们APP是否有持有一小时以上的wack_lock。  

![选择腾讯视频app](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2019-02-20-Android-Quantity-13.jpg) 

* Wakeup alarm info表格中展示了APP运行过程中触发的wakeup alarm名字和个数，通过该分析工具也可以统计app的闹钟唤醒次数。  

![选择腾讯视频app](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2019-02-20-Android-Quantity-14.jpg) 

- C.QAPM  
QAPM是SNG开发的致力于解放专项测试人员的工具平台，该平台带有电量监控功能，在电量个例菜单中会统计前台30分钟、后台5分钟两个场景下的wacklock持有信息。该平台上的数据可以作为我们电量测试的参考对象，具体的统计方法还需后续深入了解。

![选择腾讯视频app](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2019-02-20-Android-Quantity-15.jpg) 

![选择腾讯视频app](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2019-02-20-Android-Quantity-16.jpg) 

- D.dumpsys命令  
Android提供的dumpsys工具能够用于查看感兴趣的系统服务信息与状态，手机连接电脑后能够直接命令行运行adb shell dumpsys 查看电池、电量相关信息。 

	**adb shell dumpsys power**  
	
![shell](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2019-02-20-Android-Quantity-17.jpg) 

通过该条命令可以看到手机中所有的wack_lock持有信息

	**adb shell dumpsys alarm**   
	
![shell](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2019-02-20-Android-Quantity-18.jpg) 

![shell](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2019-02-20-Android-Quantity-19.jpg) 

此命令会提供设备上的alarm系统服务相关信息。其中Alarm Stats列出了应用设置alarm的情况，其中有系统被该应用所有alarm消耗的时间以及被闹钟唤醒的次数。可以通过获取一小时内的电量数据来分析用户在每小时的唤醒次数。
相关链接： <https://blog.csdn.net/memoryjs/article/details/48709183>
该方法与通过burgreport文件统计电量信息类似，都是通过Android系统中提供的工具来输出电量的消耗情况，且该种方式输出的报告也比较复杂，可读性查，可在测试过程中作为参考。

# 4、国际版电量测试方法总结与实践   

## 4.1 测试方法总结
根据上一节的测试方法研究，我们打算首先用GT测试各个场景中APP电量消耗是否有异常。
接下来采用battery historian分析工具对手机里获取的bugreport文件进行分析，统计app中持有超过一小时的wack_lock和一小时内发生的wackup数。
QAPM中采集到的数据作为我们的辅助分析数据，我们可以比较两份数据，看我们通过battery historian统计的wack_lock数据是否准确。
我们也可以通过使用dumpsys命令，查看app电量相关信息作为测试辅助方法。

## 4.2 测试方法实践  

腾讯视频国际版1.0.0已经发布，我们已经使用该方法对其进行了一次电量测试，具体测试过程如下：

- A.GT测试：

测试场景：启动-播放-前台静置   
测试机器：nexus   
测试结果分析：  

* 从以下电流趋势变化图中可以看出，播放过程和前台静置过程，电流曲线平稳，无较大波动，无明显异常。   
* 从播放到退出播放前台静置，使用电流明显变小，符合预期。   
	
![电流](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2019-02-20-Android-Quantity-19.jpg) 

- B.Battery Historian测试：

测试场景：   

	* app前台静置2小时  
	* app后台静置2小时  
	* 全屏播放2小时  
	
测试型号：   

	* Y7 Pro 2018 （LDN-LX2）  
	* OPPO F7 （CPH1819）  
	
测试结果分析：   
	* 三个场景中，仅播放场景下会持有WindowManager这个wakelock超过1小时以上。而Android Vitals中关注的是app运行在后台时，长时间持有部分唤醒锁的情况，播放这个场景可以排除在外，因此得出结论，国际版APP持有唤醒锁情况正常。

场景|机型|持有1小时以上的wack lock  
-----|-----|-----  
app前台|华为Y7 Pro 2018 （LDN-LX2）|无  
app前台|OPPO F7 （CPH1819）|无  
app后台|华为Y7 Pro 2018 （LDN-LX2）|无  
app后台|OPPO F7 （CPH1819）|无  
全屏播放|华为Y7 Pro 2018 （LDN-LX2）|WindowManager  
全屏播放|OPPO F7 （CPH1819）|WindowManager  

* 测试过程中没有统计到alarm数据，说明国际版APP暂时没有使用到AlarmManager定时任务。
	
- C.测试结论：

* GT电流测试显示国际版APP各应用场景电量使用情况正常。 
	
场景|启动APP|播放|退出播放，前台静置  
-----|-----|-----  
结论|启动过程需加载图片等资源，电流较大，正常|播放过程电流平稳无异常|退出播放电流变小，静置过程平稳无异常  

* Battery Historian分析电量数据得出，前台静置、后台静置、播放三个场景中仅播放场景会持有wack lock1小时以上，不属于Android Vitals统计范畴，不会影响到国际版APP在Google Play商店的排名。 
	
场景|机型|stuck wake locks|excessive wakeups|结论  
-----|-----|-----  
前台静置|华为Y7 Pro 2018 （LDN-LX2）|无唤醒锁定卡住|无过渡唤醒|正常  
前台静置|OPPO F7 （CPH1819）|无唤醒锁定卡住|无过渡唤醒|正常  
后台静置|华为Y7 Pro 2018 （LDN-LX2）|无唤醒锁定卡住|无过渡唤醒|正常  
后台静置|OPPO F7 （CPH1819）|无唤醒锁定卡住|无过渡唤醒|正常  
播放|华为Y7 Pro 2018 （LDN-LX2）|持有唤醒锁1小时以上|无过渡唤醒|正常  
播放|OPPO F7 （CPH1819）|持有唤醒锁1小时以上|无过渡唤醒|正常 

