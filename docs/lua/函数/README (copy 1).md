##### Lua函数
Lua函数的主要两个用途是
1. 完成某项任务
2. 计算并返回结果


##### Lua函数的定义
Lua函数的定义语法如下所示:


```lua
function_scope function function_name(parameter_list)
	-- statements
end
```

1. function_scope: 函数的作用范围,默认是全局,可使用local关键字声明为局部的
2. function: 函数声明关键字
3. function_name: 函数名字
4. parameter_list: 函数参数


Lua函数示例:


```lua
function add(x, y)
  return x + y
end
```


##### Lua函数的多参数返回
Lua中的函数是支持多参数返回的,只需在return关键字后面列出需要返回的值的列表即可.


Lua函数多参数返回



```lua
function maximum (a)
    local mi = 1
    local m = a[mi]
    for i,val in ipairs(a) do
       if val > m then
           mi = i
           m = val
       end
    end
    return m, mi
end

print(maximum({8,10,23,12,5}))
```


##### Lua可变长参数
Lua支持可变长参数,只需在参数列表中使用三个点即可`...`


Lua可变长参数示例:


```lua
function average(...)
   result = 0
   local arg={...}
   for i,v in ipairs(arg) do
      result = result + v
   end
   print("总共传入 " .. select("#",...) .. " 个数")
   return result/select("#",...)
end

print("平均值为",average(10,5,3,4,5,6))
```

Note: select函数可以获取可变长参数的格式,它的使用格式为`select("#", ...)`


---
that's all