##### Redis数据类型
redis支持5中数据类型: string, hash, list, set, zset


##### 字符串
string是redis最基本的数据类型，一个key对应一个value
并且string是二进制安全的，即redis可以包含任何数，例如png图片或序列化对象
string一个key最大能存储512MB的数据


##### Hash
Hash是一个key-value集合，是一个string类型的field和value的映射表，特别适合存储对象
每个hash可以存储2的32-1次方个键值对


##### Lists
Lists是简单的字符串列表，按照插入顺序排序。你可从左边或右边添加元素
Lists最多存储2的32-1次方个元素


##### Set
Set是一个字符串类型的无序集合
集合是通过hash表来实现的，所以添加，删除和查找的复杂度都为O(1)
Set最大存储成员的个数为2的32-1次方个


##### ZSet
ZSet和Set一样也是string类型元素的集合，且不允许重复的成员
不同的是每个元素都会关联一个double类型的分数，通过这个分数来为集合中的成员进行从小到大的排序
ZSet的成员是唯一的，但是分数却可以是重复的

---
that's all