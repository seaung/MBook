##### NSE脚本编写规则

一个完整的nse脚本包括如下几个部分

1. description字段: 这部分内容介绍了nse的功能，在nmap中可以使用--script-help选项来阅读其中的内容。
2. categories字段: 这部分的内容给出脚本所在的分类，在nmap执行脚本时可以分类执行。
3. action字段: 脚本执行的具体内容。当脚本通过rule字段的检查被触发执行时，就会调用action字段定义的函数。
4. rule字段: 该字段描述了脚本执行的规则，也就是确定触发脚本执行的条件。这个规则是一个lua函数，返回值只有true和false。只有当rule字段返回true时。action字段中的函数才会被执行

##### 一个完整的nse脚本示例

```lua
local shortport = require "shortport"

description = [[
this is a description
]]

author = "seaung"

license = "Same as Nmap--See http://nmap.org/book/man-legal.html"

categories = {"default"}

portrule = function(host, ip)
    return true
end

action = function(host, ip)
   -- do something
end
```

##### NSE脚本规则

1. prerule规则: 这个规则执行要早于nmap的扫描，因此这类脚本不会调用nmap扫描得到的结果。执行的顺序是先执行脚本，后执行nmap扫描。
2. hostrule规则: 这个规则是在nmap已经完成了主机发现之后执行的，根据主机发现的结果来触发该类脚本。执行的顺序是先执行nmap主机发现，后执行脚本。
3. portrule规则: 这个规则与hostrule()规则类似，不过是在执行了端口扫描完成后或版本探测是才会触发脚本。这个规则的执行顺序是先执行nmap端口扫描，后执行脚本。
4. postrule规则: 这个规则是在nmap已经完成所有的扫描之后才会执行，一般用来处理扫描结果。这个规则的执行顺序是当所有的扫描都结束后才会执行脚本。





----

that's all