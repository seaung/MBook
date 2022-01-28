##### Lua数组
数组就是相同数据类型的元素按一定顺序排列的集合
Lua数组的索引键值可以使用整数表示,数组的大小不是固定的.


##### Lua一维数组
一维数组是最简单的数组,其逻辑结构是线性表

```lua
arr = {"lua", "python", "js"}

for i = 0, 3 do
  print(arr[i])
end

-- nil, lua, python, js
-- lua数组的索引是从1开始的
```


##### Lua多维数组
多维数组即数组中包含数组或一维数组的索引键对应一个数组

```lua
arr = {}

-- 初始化数组
for i = 1, 3 do
  arr[i] = {} -- 一维数组的索引键对应一个数组
  for j = 1, 3 do
    arr[i][j] = i * j
  end
end

-- 遍历数组
for i = 1, 3 do
  for j = 1, 3 do
    print(arr[i][j])
  end
end
```


---
that's all