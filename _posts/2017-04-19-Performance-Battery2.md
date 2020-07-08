---
layout: post
category: 工具
title: 电量统计(2)-日志
tagline: batterystats
tags: Android
---

# 概要

在[电量统计(1)-原理](/2017-04-19-Performance-Battery1)一文
中，我们分析了电量统计服务的运行机制、耗电量的计算方法。本文我们分析电量统计的输出日志，包括日志信息的格式、表示的意义等，这些日志信息能够帮助开发人员解决一些功耗和性能问题。

# 电量统计信息的分析

Android提供的**dumpsys**命令用于查看系统服务的信息(实现原理可以查阅[dumpsys介绍](/2017-04-18-Performance-dumpsys))，
将**batterystats**作为参数，就能输出完整的电量统计信息。

{% highlight console %}
$ adb shell dumpsys batterystats
Battery History (2% used, 6184 used of 256KB, 36 strings using 2418):
                    0 (9) RESET:TIME: 2015-07-23-07-59-11
                    0 (2) 100 status=not-charging health=good plug=none temp=310 volt=4308 +running +wake_lock +audio +screen +phone_in_call phone_signal_strength=great +wifi_running +wifi wifi_signal_strength=4 wifi_suppl=completed top=u0a4:"com.android.dialer"
                    0 (2) 100 user=0:"0"
                    0 (2) 100 userfg=0:"0"
             +1s868ms (2) 100 phone_signal_strength=very
             +4s655ms (3) 100 -phone_in_call -top=u0a4:"com.android.dialer"
                  ...
          +5m10s450ms (2) 100 phone_signal_strength=great wifi_signal_strength=3
          +5m24s232ms (2) 100 -top=u0a13:"com.android.mms"
          +5m24s232ms (2) 100 +top=u0a10:"com.android.launcher3"
          +5m29s113ms (2) 099
          +5m31s160ms (2) 099 -top=u0a10:"com.android.launcher3"
          +5m31s160ms (2) 099 +top=u0a2:"com.android.browser"
                  ...
         +21m20s645ms (2) 099 +wake_lock=1000:"*alarm*:android.intent.action.TIME_TICK"
         +21m20s664ms (1) 099 -wake_lock
         +21m44s890ms (2) 098
         +24m41s381ms (3) 098 +wake_lock=-1:"screen" +screen brightness=dim
         +24m50s491ms (1) 098 wifi_signal_strength=3
                  ...
         +34m22s702ms (2) 098 temp=320
         +39m23s248ms (1) 098 -running
         +42m28s514ms (2) 098 +running +screen -phone_in_call
         +42m28s808ms (3) 097 data_conn=umts wifi_suppl=scanning
                  ...
         +55m49s554ms (3) 097 +wake_lock=-1:"screen" +screen brightness=bright
         +55m49s750ms (2) 097 brightness=dark
         +55m50s000ms (2) 096 temp=330
         +55m51s998ms (2) 096 brightness=dim
                  ...
         +59m02s534ms (2) 096 +top=u0a10:"com.android.launcher3"
         +59m03s452ms (2) 096 -top=u0a10:"com.android.launcher3"
         +59m03s452ms (2) 096 +top=u0a2:"com.android.browser"
         +59m19s915ms (2) 095
         +59m49s908ms (2) 095 volt=4190
       +1h01m50s806ms (2) 095 data_conn=umts
       +1h01m57s536ms (2) 095 data_conn=hspap
       +1h02m04s176ms (2) 095 data_conn=umts
       +1h02m12s472ms (2) 095 data_conn=hspap
                  ...
{% endhighlight %}

本文引用至 <http://duanqz.github.io/2015-07-21-batterystats-part2>.
