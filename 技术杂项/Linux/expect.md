##### 简介

最近工作中遇到这样的需要，需要使用easy-rsa来自动生成某些证书，生成证书过程中需要用户手动输入一些信息。大佬对我说，小伙你试试将这个过程变成自动化的，于是就有了这篇东西了。就当小总结吧。

##### 什么是expect?

expect是linux下的一个自动化交互套件，主要用于执行命令或程序时，系统以交互式形式要求用户输入指定字符，实现通信。

##### expect自动交互流程

```bash
spawn启动指定进程--->expect获取匹配指定关键字--->send向指定程序发送指定字符--->执行完成退出
```

##### expect常用指令

| 指令            | 描述                                           |
| :-------------- | :--------------------------------------------- |
| spawn           | 交互程序开始后面跟命令或者指定程序             |
| expect          | 获取匹配信息匹配成功则执行expect后面的程序动作 |
| send， exp_send | 用于发送指定的字符串信息                       |
| send_user       | 用来打印输出 相当于shell中的echo               |
| exit            | 退出expect脚本                                 |
| eof             | expect执行结束 退出                            |
| set             | 定义变量                                       |
| puts            | 输出变量                                       |
| set timeout     | 设置超时时间                                   |
| interact        | 允许用户交互                                   |

##### 例子

```bash
#！/usr/bin/except

spawn sudo pacman -S masscan
expect "\[sudo\] password for*" {send "123456\r"; exp_continue;}

send_user "end\n"
```



----

that's all