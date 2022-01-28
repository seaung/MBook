##### Lua中的变量
在Lua中有三种类型的变量
1. 全局变量
2. 局部变量
3. 表中的域

```lua
a = 5; -- 全局变量

local b = 5; -- 局部变量

function hello()
  c = 5; -- 全局变量
  local d = 6; -- 局部变量
end

hell()
print(c, d) -- 5, nil
```


---
that's all