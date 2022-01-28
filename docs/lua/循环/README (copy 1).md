##### Lua循环结构
Lua中的循环有很多种,例如while,for,repeat-until等


##### while循环
while循环的特点是先判断真假,后执行后续的逻辑.格式如下所示

```lua
while (condition)
do
	statements
end
```

示例

```lua
a = 10

while (a < 20)
do
  print("a current value of : ", a)
  a = a + 1
end
```


##### for循环
Lua中的for循环分为两大类
1. 数值for循环
2. 泛型for循环


数值for循环语法格式
```lua
for var=exp1, exp2, exp3 do
  statements
end
```


示例
```lua
function f(x)
  return x * 2
end

for i = 1, f(5) do
  print(i)
end
```


泛型for循环语法格式
```lua
for key, value in ipairs(..) do
  print(key, value)
end
```


示例
```lua
a = {"one", "tow", "three"}

for key, value in ipairs(a) do
  print(key, value)
end
```


#### repeat-until循环
repeat-until跟C中的do-while循环类似都是先执行循环语句后判断循环条件.它的语法格式如下所示


```lua
repeat
  statements
until(condition)
```


示例
```lua
a = 10

repeat
  print("a current value of : ", a)
  a = a + 1
until(a > 15)
```


##### 循环控制语句
Lua支持两种循环控制语句
1. break语句
2. goto语句
Note: Lua没有continue语句,通常情况下,如果需要使用类似continue的功能,我们都会使用goto来替代


break语句示例
```lua
a = 10;
while(true) do
  print(a)
  if (a == 15) then
  	break
  end
end
```


goto语句示例
```lua
local a = 1;

::label:: print("goto") -- goto的语法格式为: ::label_name::

a = a + 1
if a < 3 then
  goto lable
end
```


---
that's all