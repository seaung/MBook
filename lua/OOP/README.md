##### Lua面向对象
Lua中的表就是一个对象.表与对象一样,可以拥有状态.拥有一个与其值无关的标识(self)
两个具有相同值的对象(表)是两个不同的对象,而一个对象可以具有多个不同的值.表与对象一样,具有与创建者和被创建者位置无关的生命周期.

```lua
Account = {balance = 0} -- 使用表模拟类,并设置属性balance的值为零

function Account.withdraw(n) -- 定义行为
    Account.balance = Account.balance - v
end

Account.withdraw(12.01) -- 
```

Note:
上述代码中withdraw方法只能针对特定对象工作.这个方法只有在对象保存在特定的全局变量中才能工作,如果我们改变了对象的名称,withdraw就不能工作了.这种行为违反了对象拥有独立生命周期的原则.

```lua
a, Account = Account, nil
a.withdraw(10.0) -- Error!
```

如果想要解决上述的那种情况,可以对操作的接受者进行操作.这是需要一个额外的参数来表示该接受者,这个参数通常被称为self或this

```lua
function Account.withdraw(self, n)
    self.balance = self.balance - n
end

a = Account; Account = nil
a.withdraw(10) -- success!
```

使用self参数是所有面向对象语言的核心点.大多数面向对象语言都向程序员隐藏了这个机制,从而使得程序员不必显示地声明这个参数.
同样的我们还可以使用冒号(:)操作符来隐藏该参数.冒号的作用是在一个方法调用中增加一个额外的实参,或在方法的定义中增加一个额外的隐藏参数.
冒号只是一种语法机制,并没有引入任何新的东西.

```lua
Account = {balance = 0, 
	withdraw = function(self, n)
					self.balance = self.balance - n
	           end
}

function Account:deposit(value)
	self.balance = self.balance + value
end

Account.deposit(10)
Account:withdraw(10)
```

##### 类
Lua语言中没有类的概念.虽然元表的概念在某种程度上与类的概念相似,但是把元表当作类使用在后续会比较麻烦
相对的,Lua可以采用原型的模式来模拟.即每个对象都可以有一个原型.原型也是一种普通对象,当对象遇到一个未知的操作时会首先在原型中查找.

```lua
function Account:new(o)
	o = o or {}
	self.__index = self
	setmetatable(o, self)
	return o
end
```


##### 继承
由于类也是对象,因此它们也可以从其他类获得方法.(通过原型来查找)

```lua
Account = {balance = 0}

function Account:new(o)
	o = o or {}
	self.__index = self
	setmetatable(o, self)
	return o
end

function Account:deposit(value)
	if value > self.balance then
		error "insufficent funds!!!"
	end
	self.balance = self.balance - value
end

function Account:withdraw(value)
	if value > self.balance then
		error "insufficent funds!!!"
	end
	self.balance = self.balance + value
end

SpecicalAccount = Account:new() -- SpecicalAccount继承了Account

s = SpecicalAccount:new{limit=100.10} -- 这重新调用new时,self指向的是SpecicalAccount

-- s的元表是SpecicalAccount,其中字段__index的值也是SpecicalAccount.因此,s继承自SpecicalAccount,而SpecicalAccount继承自Account

function SpecicalAccount:withdraw(value) -- 重新定义从基类继承的(任意)方法
	if value - self.balance > self:getLimit() then
		error "insufficent funds!!!"
	end
	self.balance = self.balance - v
end

function SpecicalAccount:getLimit()
	return self.limit or 0
end
```

Note:
在lua中的对象有个比较有趣的特性,就是无须为了指定一种新行为而创建一个新的类.如果只有单个对象需要某中特殊行为,那么我们可以直接在该对象中实现该行为.

```lua
function s:getLimit()
	return self.balance * 0.01
end
```


---
that's all