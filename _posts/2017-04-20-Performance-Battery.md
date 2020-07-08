---
layout: post
category: 测试
title: APP电量消耗测试
tagline: [battery, batterystats]
tag: 性能测试
---

测试一款APP导致手机发热异常严重，从而想要看一下这款APP所耗电量是一个什么情况，于是就有了下面测试APP电能消耗的相关数据收集。

# 1. battery

通过下面的命令可以获取目前手机(SAMSUNG S6)电池信息。

    adb shell dumpsys battery

得到如下返回数据:

    Current Battery Service state:
		mBootCompleted: true
		AC powered: false
		USB powered: true
		Wireless powered: false
		Max charging current: 0
		status: 2                              //当前电池状态
		health: 2                              //当前电池健康
		present: true                          //是否提供电池功能；有些手机在使用USB电源的情况下，即使拔出了电池，仍然可以正常工作
		level: 16                              //当前电池剩余容量%
		scale: 100                             //电池最大值；通常为100
		voltage: 3805                          //当前电池电压；单位mV
		temperature: 289                       //当前电池温度；(289/10)℃
		technology: Li-ion                     //当前电池技术；比如，对于锂电池是Li-ion
		batterySWSelfDischarging: false
		batterySecEvent: 0
		LED Charging: false
		LED Low Battery: false
		current now: 325
		Adaptive Fast Charging Settings: true
	BatteryInfoBackUp
	  	mSavedBatteryAsoc: 96
	 	mSavedBatteryMaxTemp: 677
	 	mSavedBatteryMaxCurrent: 2638
	  	mSavedBatteryUsage: 44991
	  	FEATURE_SAVE_BATTERY_CYCLE: true
	USE_FAKE_BATTERY: false
	SEC_FEATURE_BATTERY_SIMULATION: false
	FEATURE_WIRELESS_FAST_CHARGER_CONTROL: false
	  	mWasUsedWirelessFastChargerPreviously: false
	  	mWirelessFastChargingSettingsEnable: true

# 2. batterystats

    adb shell dumpsys batterystats > d:\batterystats.txt

通过文本编辑器打开刚刚保存的文件，并查看Estimated power use (mAh)相关信息，并查看对应uid的耗电信息。 通过batterystats 在Android 6.0以上已可以获取具体APP所消耗的电量值。  

    Estimated power use (mAh):
	    Capacity: 3000, Computed drain: 436, actual drain: 300-360
	    Uid 0: 157 ( cpu=50.2 wake=107 wifi=0.00128 )
	    Idle: 154
	    Uid 1000: 45.6 ( cpu=43.4 wake=0.616 wifi=0.142 gps=0.228 sensor=1.21 )
	    Screen: 22.3
	    Cell standby: 14.2 ( radio=14.2 )
	    Uid u0a155: 11.2 ( cpu=11.0 wifi=0.0000144 sensor=0.00107 camera=0.152 )
	    Uid u0a146: 6.82 ( cpu=6.71 wake=0.00399 wifi=0.000565 sensor=0.107 )
	    Wifi: 6.28 ( cpu=0.359 wifi=5.92 )
	    Uid 9802: 4.48 ( cpu=2.64 wake=1.54 wifi=0.305 )
	    Uid 1036: 3.89 ( cpu=3.89 )
	    Uid u0a148: 1.76 ( cpu=0.781 wake=0.982 wifi=0.0000860 )
	    Uid 1001: 1.73 ( cpu=1.72 wake=0.0148 )
	    Uid u0a10: 1.17 ( cpu=1.17 wifi=0.0000860 )
	    Uid u0a102: 0.475 ( cpu=0.467 wake=0.00818 wifi=0.000203 )
	    Uid 1023: 0.473 ( cpu=0.473 )
	    Uid u0a68: 0.445 ( cpu=0.0219 wake=0.000160 wifi=0.423 )
	    Uid u0a54: 0.366 ( cpu=0.248 wake=0.118 )
	    Uid u0a74: 0.246 ( cpu=0.246 wifi=0.0000493 )
	    Uid u0a157: 0.189 ( cpu=0.189 )
	    Uid u0a110: 0.163 ( cpu=0.161 wake=0.00143 wifi=0.000336 )   

可以通过uid对应到指定的APP，可以获取到对应APP所消耗的电量，并且可以查看到电量主要消耗在哪些部分。

# 3. 电量数据统计
获取了batterystats的数据后，要连续不间断获取对应APP电量消耗的曲线图以查看对应APP电量消耗态势。

# 4. historian数据分析
1. 下载historian.py脚本，下载地址:<https://github.com/google/battery-historian>.
1. 执行步骤:  
>1)初始化batterystats数据  
>`adb shell dumpsys batterystats --reset`  
>2）拔掉手机，操作app，操作完成后，重新连接手机，执行下面的命令，收集系统整体的Battery数据:  
>`adb shell dumpsys batterystats > batterystats.txt`  
>3）得到这些数据后，这个时候使用我们的battery-historian来生成我们可见HTML报告:  
>`Python historian.py batterystats.txt > batterystats.html`  
>4）用浏览器打开此文件即可  

通过脚本historan.py生成对应的HTML可视化数据。  
![batterystats可视化数据](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/1-2017-04-20-Performance-Battery.png)  
[Check HTML](./resource/Test/batterystats-2017-04-20-Performance-Battery.html)

# 5. 电量统计小工具
>## 1.PowerTutor
>Google强大的功耗测试工具。1.显示系统电量消耗水平，包括LCD/OLED，CPU,WiFi，3G，GPS和Audio；2.查看某段时间内所有运行中的应用程序的耗电量。
>详见<http://ziyang.eecs.umich.edu/projects/powertutor/>。
>## 2.Battery Monitor Widget  
>高度可定制化的电量监控小工具，不仅可以显示当前电量，估算剩余电量支撑时间，还可以一目了然地检测出各个APP的耗电历史，从而方便进行比较。详见<https://play.google.com/store/apps/details?id=ccc71.bmw>。
>## 3.Smart Battery Monitor  
>除了可以在状态栏显示电池消耗百分比外，还可以显示电池的温度，以及已充电时间。详见<https://play.google.com/store/apps/details?id=at.aauer1.battery>。
>## 4.Battery Indicator  
>亮点是轻便、小巧，甚至使用它时不会消耗电池。当然，它可以显示电池的电量百分比、电池的温度、以及电池的健康信息。详见<https://play.google.com/store/apps/details?id=com.darshancomputing.BatteryIndicator>。
>## 5.Battery Widget  
>这个工具不仅是一个电池Widget，还可以作为显示选项，GPS，WiFi，蓝牙选项等的快捷方式。详见<https://play.google.com/store/apps/details?id=com.geekyouup.android.widgets.battery>。
>## 6.Battery Saver
>被称为管理电池的最强大应用之一。除了显示电量信息，电池温度和健康信息外，它还可以快速管理一些耗电量大的应用如：GPS，WiFi，蓝牙等。另外：该工具可以查看一天之内的哪个时间点电池的耗电量最大。详见<https://play.google.com/store/apps/details?id=com.antutu.powersaver>