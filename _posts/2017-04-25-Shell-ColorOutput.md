---
layout: post
category: 工具
title: Shell带颜色输出
tagline: Shell
tag: 工具
---

如何在Linux下Shell脚本输出带颜色文字：  
    echo -e "\033[44;37;5m ME \033[0m"  
![Output](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Tools/1-2017-04-25-Shell-ColorOutput.gif)   
以上命令设置作用如下：背景色为蓝色，前景色为白色，字体闪烁，输出字符ME，然后重新设置屏幕到缺省设置，输出字符me后颜色回复正常。

- e ：是echo的一个可选项，它用于激活特殊字符的解析器。
- \033 ：引导非常规字符序列。意味着设置属性然后结束非常规字符序列，这个例子里真正有效的字符是44;37;5。
- 44;37;5和0 ： 修改44;37;5可以生成不同颜色的组合，数值和编码的前后顺序没有关系

可以选择的编码如下所示：

编码|颜色/动作   
-----|-----  
0|重新设置属性到缺省设置| 
1|设置粗体
2|设置一半亮度(模拟彩色显示器的颜色)
4|设置下划线(模拟彩色显示器的颜色)  
5|设置闪烁  
7|设置反向图象  
22|设置一般密度  
24|关闭下划线  
25|关闭闪烁  
27|关闭反向图象  
30|设置黑色前景  
31|设置红色前景  
32|设置绿色前景  
33|设置棕色前景  
34|设置蓝色前景  
35|设置紫色前景  
36|设置青色前景  
37|设置白色前景  
38|在缺省的前景颜色上设置下划线  
39|在缺省的前景颜色上关闭下划线  
40|设置黑色背景  
41|设置红色背景   
42|设置绿色背景  
43|设置棕色背景   
44|设置蓝色背景  
45|设置紫色背景   
46|设置青色背景  
47|设置白色背景   
49|设置缺省黑色背景  

代码示例:

```shell
#!/bin/sh
#  Author:    Tester
#comments:    Start the services and the sites with shell
#    Date:    2016-05-11
#function:    重启tomcat中在执行或未执行的程序或服务示例.
#获取配置文件中需要重启的程序，记录示例：on    /tomcat/tomcat_TestService | off    /tomcat/tomcat_TestService
service_conf="/tomcat/tomcat_conf/service.config"
count=1
cat ${service_conf} | grep -v "^#" | while read is_on service_dir
do
    exe_start_dir="${service_dir}/bin/startup.sh"
    exe_shutdown_dir="${service_dir}/bin/shutdown.sh"
    if [[ ${is_on} = "on" ]]
    then
        num=$(ps -ef | grep ${service_dir} | grep -v "grep" | wc -l)    
        echo -e "\033[41;32;1m共有${num}个运行进程:${service_dir}\033[0m"
        if [[ ${num} -ge 1 ]]
        then
            echo -e "\033[40;32;1m结束已存在的进程:\033[0m"        
            bash $exe_shutdown_dir
            echo -e "\033[40;32;1m重新启动程序${service_dir}:\033[0m"
            sleep 5
            bash $exe_start_dir
        else
            if [[ ${num} -gt 1 ]]
            then
                echo -e "\033[47;32;1m存在${num}个运行的同一线程，结束端口号为：${tmp2}的进程：\033[0m"
                kill ${tmp2}
            else                    
                echo -e "\033[40;32;1m程序${service_dir}未启动，现启动程序...\033[0m"
                    bash $exe_start_dir
            fi
        fi
        result_num=$(ps -ef | grep ${service_dir} | grep -v "grep" | wc -l)
        echo -e "\033[40;34;1m***************Query Result : ${result_num}***********************\033[0m"
        ps -ef | grep ${service_dir} | grep -v "grep"
        echo -e "\033[40;34;1m************************************************************\033[0m"
    else        
        echo -e "\033[40;34;1m*************************** Tips ${count}**************************\033[0m"
        echo -e "\033[40;31;1mThis program ${service_dir} is turned off.\033[0m"
        echo -e "\033[40;34;1m************************************************************\033[0m"
        ((count++))
    fi
done  
```