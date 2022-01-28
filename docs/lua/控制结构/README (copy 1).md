##### Lua流程控制语句
Lua提供了两种流程判断控制语句,分别为if和if-else这两种.


##### if语句
if语句语法格式如下:
```lua
if(boolean_condition) then
	statements
end
```


示例:
```lua
a = 10

if (a > 12) then
  print("ho")
end
```


##### if-else语句
if-else语句语法格式如下所示:
```lua
if (boolean_condition) then
  statements
else
  statements
end
```


示例:
```lua
a = 10

if (a > 12) then
  print("ho")
else
  print("ha")
end
```


if-elseif-else语句语法格式如下所示:
```lua
if (boolean_condition) then
  statements
elseif (boolean_condition) then
  statements
else
  statements
end
```


示例:
```lua
a = 10

if (a == 10) then
  print(a)
elseif (a > 10) then
  print(a)
else
  print(a)
end
```


---
that's all