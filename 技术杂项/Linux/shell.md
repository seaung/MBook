##### 前言

最近工作上老是用到shell脚本，所以就有了这篇笔记了

##### 定义变量

```shell
vars="hello world"
vars1=hello # 如果有空格，则使用变量是会出错
```

##### 使用变量

```shell
echo $vars
echo ${vars}
# 根据使用场景来使用
```

##### 修改变量

```shell
ars=123
echo $ars
ars=hello # 修改变量
```

##### 单引号和双引号

1. 使用单引号包裹的字符串会被原样输出
2. 双引号可以嵌入变量

##### 将命令执行结果赋值给变量

```shell
vars=`pwd` # 在多种shell下有效
echo $vars
vars=$(pwd) # 只在bash shell有效
echo $vars

start=`date +%s` # 获取时间戳
echo hello
end=$(date +%s)
total=$((end - start)) # (())为shell数学计算命令
```

##### 只读变量

```shell
url="https://google.com"
readonly url
echo $url
```

##### 删除变量

```shell
unset vars # unset不能删除只读变量
```

##### 给shell脚本传递参数

```shell
#!/bin/bash
echo $0
echo $1
# ${n} 从命令行中传递参数给脚本，$0为脚本名称，$1为第一个参数往后依次类推，n>10之后需要使用${n}来获取
```

##### shell特殊变量

| 变量     | 描述                               |
| -------- | ---------------------------------- |
| $0       | 当前shell脚本名称                  |
| $n(n>=1) | 传递给脚本或函数的参数             |
| $#       | 传递给脚本或函数的参数的长度       |
| $*       | 传递给脚本或函数的所有参数         |
| $@       | 传递给脚本或函数的所有参数         |
| $?       | 上个命令的退出状态，或函数的返回值 |
| $$       | 当前 Shell 进程 ID                 |

##### $@和$*的区别

1. 如果它们不被双引号包裹时，它们直接没啥区别
2. 如果它们被双引号包裹时，就有了区别了，"$@"将每个参数当作一份数据，并且彼此之间是独立的。"$*"会将所有的参数从整体上看作一份数据，而不是把每个参数都看作一份数据

##### $?获取上个命令的退出状态

成功返回0,失败返回1

##### 流程控制

1. if

```shell
if condition;then
...
if

if condition;then
...
else
fi

if condition;then
...
elif condition;then
...
else
...
fi
```

2. for

```shell
for var in ietm ...
do
...
done
```

3. while

```shell
while condition
do
...
done
```

4. case

```
case value in
mod)
...
;;
mod)
...
;;
...
esac
```

##### 数组

1. 定义数组

```shell
arr=(1 2 3) # 元素间用空格隔开
arr[3]=34
```

2. 获取数组元素

```shell
echo ${arr[0]}
```

3. 获取数组所有元素

```shell
echo ${arr[@]}
echo ${arr[*]}
```

4. 获取数组长度

```shell
echo ${#arr[*]}
echo ${#arr[@]}
```

5. 数组拼接

```shell
arr_new=(${arr1[@]} ${arr2[@]})
arr_new=(${arr1[*]} ${arr2[*]})
```

6. 删除数组元素

```shell
unset arr[2] # 删除索引为2的元素
unset arr#删除整个数组
```

##### 函数

1. 函数定义

```shell
function func_name(){
...
}
```

2. 函数的调用

```shell
function_name
```

3. 函数参数

```shell
function_name param1 ....
```

4. 函数返回值的获取

Shell 函数的返回值只能是一个介于 0~255 之间的整数，其中只有 0 表示成功，其它值都表示失败。

```shell
# 使用全局变量
#!/bin/bash
sum=0  #全局变量
function getsum(){
    for((i=$1; i<=$2; i++)); do
        ((sum+=i))  #改变全局变量
    done
    return $?  #返回上一条命令的退出状态
}
read m
read n
if getsum $m $n; then
    echo "The sum is $sum"  #输出全局变量
else
    echo "Error!"
fi
# 这种方案的弊端是：定义函数的同时还得额外定义一个全局变量，如果我们仅仅知道函数的名字，但是不知道全局变量的名字，那么也是无法获取结果的

# 使用$()
#!/bin/bash
function getsum(){
    local sum=0  #局部变量
    for((i=$1; i<=$2; i++)); do
        ((sum+=i))
    done
   
    echo $sum
    return $?
}
read m
read n
total=$(getsum $m $n)
echo "The sum is $total"

# 这种方案的弊端是：如果不使用$()，而是直接调用函数，那么就会将结果直接输出到终端上
```



----

that's all