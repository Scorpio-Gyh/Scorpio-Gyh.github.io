---
title: lua
date: 2024-05-27 15:30:41
tags: 游戏引擎
---
# 注释

* 注释：--
* 多行注释: --[[           ]]--

# 数据类型

| 类型     |                       解释                       |
| -------- | :-----------------------------------------------: |
| nil      |           同null，在条件表达式中同false           |
| boolean  |                    false true                    |
| string   |  单行：用单引号、双引号<br />多行：用[[ ]]括起来  |
| table    |                类似数组，但更强大                |
| function |                       函数                       |
| thread   |                     协同线程                     |
| userdata | 自定义数据类型，一般由C/C++或应用程序所创建的类型 |

# 保留字

| and    | break  | do   | else     | elseif |
| ------ | ------ | ---- | -------- | ------ |
| end    | false  | for  | function | if     |
| in     | local  | nil  | not      | or     |
| repeat | return | then | true     | until  |
| while  | goto   |      |          |        |

除此之外，Lua定义了很多内置全局变量以 _ + 大写字母构成，如_ABC

# 变量

Lua为弱类型语言，变量无需声明

Lua变量为动态类型，可随时改变类型

变量默认为全局变量

局部变量以 local 声明

# 运算符

大部分与C++相同，需要具体注意的：

^  幂乘

// 整除  在5.3版本后可用

~= 不等于

and or not分别对应 && 、|| 、！与或非

.. 字符串连接符   a..b即连接

#返回字符串长度或表长度

# 函数

以function开头，以end结尾

## 固定参数函数

```lua
function f (a,b)
	print(a,b)
end
```

下列调用均合法，若多于定义，则舍弃，若少于定义，则以nil补充

```lua
f()
f(0)
f(0,0)
f(0,0,0)
```

## 可变参数

```lua
function f(...)
	local a,b,c,d = ...
	print (a,b,c,d)
```

## 多返回值

```lua
function f(a,b)
	return a,b
end

m,n = f(3,5)
```

## 函数为参数

直接传递即可

```lua
function sum(a,b)
	return a+b
end

function f(m,n,fun)
	local res = fun(m,n)
	print(res)
end
```

可以传递匿名函数

```lua
f(3,5,
function(a,b)
	return a+b
end
)
```

# 流程控制语句

## if 语句

if..then表示条件判断

false，nil为假

true，非nil为真（0也是真）

```lua
if a > 0 then
	print("")
elseif then
	print("")
else
	print("")
end
```

# 循环语句

## while..do

```lua
while a > 0 do
	print(a)
end
```

## repeat..until

```lua
repeat
	print(a)
	a = a - 1
until a <= 0
```

## for循环

意为for(int i = 10;i <= 50 ;)

```lua
for i = 10, 50 ,20 do
	print(i)
end
```

## break同C++

## goto语句

实现循环

```lua
function f(a)
	::flag:: print("------")
	if(a > 0) then
		print(a)
		a = a - 1
		goto flag
	end
end
```

# Table

## 数组

### 一维数组

```lua
cities = {"北京","上海","广州"}

cities[4] = "深圳"

for i=1,4 do
    print("cities["..i.."] = "..cities[i])
end
```

### 二维数组

```lua
arr = {}

for i = 1,3 do
    arr[i] = i
    for j = 1,3 do
        arr[j] = j
    end
end
```

## Map

### 基本结构

允许Map有数组和Map的嵌套

```lua
info = {name = "WYK",age = 16}
info["gender"] = "男"
info.office = "深圳"

a = "xxx"
info[a.."bb"] = true -- info["xxxbb"] = true用表达式作为key

```

## 操作符

### table.concat(table,split,start,end)

table：被连接的数组

split：连接符

start：起始位置

end：终止位置

返回连接后的数据，常用于print

### table.unpack(table,i,j)

返回数组第i到j个元素值

### table.pack(a,b,c)

将参数打包为一个数组，并存储了一个属性值n，表示数组的元素个数

### table.maxn(table)

返回最大的元素下表，即个数

### table.insert(table,pos,value)

在table的pos处插入value

### table.sort()

默认按照升序排序，但可传递排序函数

```lua
t = {8, 2, 6, 3, 7}
table.sort(t, function(a, b) return a > b end) -- 降序排序
for k, v in pairs(t) do
    print(v)
end
```

# 迭代器

## pairs

用于遍历表的所有元素的迭代器。它返回两个值：键和值。这个迭代器不保证元素的顺序

```lua
t = {name = "WYK", age = 16, gender = "男", office = "深圳"}
for k, v in pairs(t) do
    print(k, v)
end
```

## ipairs

用于遍历数组部分（即键为整数的部分）的迭代器。它返回两个值：索引和值。这个迭代器按照索引的升序遍历元素，从1开始，直到遇到第一个nil值。

```lua
t = {"apple", name = "banana", "cherry",["1"] = "pairs"}
for i, v in ipairs(t) do
    print(i, v)
end
```

这里只会输出apple和banana

# 模块

wykrec.lua文件：

```lua
rectangle = {}
rectangle.pi = 3.14
function rectangle.perimeter(a,b)
    return (a + b) * 2
end

rectangle.area = function(a,b)
    return a * b
end

aaawww = 1
return rectangle
```

main.lua文件：

```lua
 require "wykrec"

print(aaawww)
print(rectangle.pi)
print(rectangle.perimeter(2,3))
print(rectangle.area(2,3))
```

会得到如下输出：

```
1
3.14
10
6
```

模块可以实现快速的代码复用，其导入方式类似TS 的export

# 闭包

闭包是一种特殊类型的函数，它可以访问其外部作用域中的变量，即使这个函数在其外部作用域已经结束的情况下也是如此

```lua
function outerFunction(x)
    local y = 10
    return function(z)  -- 这个匿名函数就是一个闭包
        return x + y + z
    end
end

local closure = outerFunction(5)
print(closure(3))  -- 输出18
```

# `outerFunction`函数返回了一个匿名函数，这个匿名函数就是一个闭包。这个闭包可以访问 `outerFunction`的局部变量 `x`和 `y`，即使在 `outerFunction`已经返回之后。当我们调用 `closure(3)`时，它会返回 `x + y + z`的结果，即 `5 + 10 + 3`，所以输出是18。

# 元表和元方法

* 元表（metatable）是一种特殊的表，它可以改变另一个表的行为。元表可以定义一些特殊的函数，这些函数被称为元方法，元表即table的元数据表
* 元方法可以改变表的一些基本行为，例如算术运算（`+`，`-`，`*`，`/`等）、比较运算（`==`，`<`，`>`等）、长度运算（`#`）、索引运算（`[]`）等。当你对一个表执行这些操作时，Lua会查找这个表的元表，如果元表中定义了对应的元方法，Lua就会调用这个元方法。

```lua
local t1 = {value = 5}
local t2 = {value = 3}

local mt = {
    __add = function(a, b)
        return {value = a.value + b.value}
    end
}

setmetatable(t1, mt)
setmetatable(t2, mt)

local t3 = t1 + t2
print(t3.value)  -- 输出8
```

## 重要函数

```lua
setmetatable(table,metatable) --为table制定元表为metatable
getmetatable(table)	      --获得table的元表
```

## 元方法与运算符的对应关系：


* `__add(a, b)`：对应 `+`运算符。当你尝试对两个表进行加法运算时，Lua会调用这个元方法。这个元方法接受两个参数，返回它们的和。
* `__sub(a, b)`：对应 `-`运算符。当你尝试对两个表进行减法运算时，Lua会调用这个元方法。这个元方法接受两个参数，返回它们的差。
* `__mul(a, b)`：对应 `*`运算符。当你尝试对两个表进行乘法运算时，Lua会调用这个元方法。这个元方法接受两个参数，返回它们的积。
* `__div(a, b)`：对应 `/`运算符。当你尝试对两个表进行除法运算时，Lua会调用这个元方法。这个元方法接受两个参数，返回它们的商。
* `__mod(a, b)`：对应 `%`运算符。当你尝试对两个表进行取模运算时，Lua会调用这个元方法。这个元方法接受两个参数，返回它们的余数。
* `__pow(a, b)`：对应 `^`运算符。当你尝试对一个表进行幂运算时，Lua会调用这个元方法。这个元方法接受两个参数，返回第一个参数的第二个参数次方。
* `__unm(a)`：对应一元 `-`运算符。当你尝试对一个表进行取反运算时，Lua会调用这个元方法。这个元方法接受一个参数，返回它的负数。
* `__concat(a, b)`：对应 `..`运算符。当你尝试对两个表进行连接运算时，Lua会调用这个元方法。这个元方法接受两个参数，返回它们的连接。
* `__len(a)`：对应 `#`运算符。当你尝试获取一个表的长度时，Lua会调用这个元方法。这个元方法接受一个参数，返回它的长度。
* `__eq(a, b)`：对应 `==`运算符。当你尝试判断两个表是否相等时，Lua会调用这个元方法。这个元方法接受两个参数，如果它们相等，返回 `true`，否则返回 `false`。
* `__lt(a, b)`：对应 `<`运算符。当你尝试判断一个表是否小于另一个表时，Lua会调用这个元方法。这个元方法接受两个参数，如果第一个参数小于第二个参数，返回 `true`，否则返回 `false`。
* `__le(a, b)`：对应 `<=`运算符。当你尝试判断一个表是否小于或等于另一个表时，Lua会调用这个元方法。这个元方法接受两个参数，如果第一个参数小于或等于第二个参数，返回 `true`，否则返回 `false`。
* `__index(a, b)`：当你尝试访问一个不存在的字段时，Lua会调用这个元方法。这个元方法接受两个参数，返回它们的索引。
* `__newindex(a, b, c)`：当你尝试修改一个不存在的字段时，Lua会调用这个元方法。这个元方法接受三个参数，设置它们的新索引。
* `__call(a, ...)`：当你尝试调用一个表时，Lua会调用这个元方法。这个元方法接受一个或多个参数，返回它们的调用结果。
* `__tostring(a)`：当你尝试将一个表转换为字符串时，Lua会调用这个元方法。这个元方法接受一个参数，返回它的字符串表示。
* `__pairs(a)`：当你尝试使用 `pairs`函数遍历一个表时，Lua会调用这个元方法。这个元方法接受一个参数，返回它的迭代器函数。
* `__ipairs(a)`：当你尝试使用 `ipairs`函数遍历一个表时，Lua会调用这个元方法。这个元方法接受一个参数，返回它的迭代器函数。
* `__metatable(a)`：当你尝试获取或设置一个表的元表时，Lua会调用这个元方法。这个元方法接受一个参数，返回它的元表。
* `__gc(a)`：当一个表被垃圾回收时，Lua会调用这个元方法。这个元方法接受一个参数，执行它的垃圾回收操作。

```lua
-- 创建两个表local t1 = {value = 5, a = 1}
local t2 = {value = 3, b = 2}

-- 创建元表
local mt = {
    -- 算术元方法
    __add = function(a, b) return {value = a.value + b.value} end,
    __sub = function(a, b) return {value = a.value - b.value} end,
    __mul = function(a, b) return {value = a.value * b.value} end,
    __div = function(a, b) return {value = a.value / b.value} end,
    __mod = function(a, b) return {value = a.value % b.value} end,
    __unm = function(a) return {value = -a.value} end,
    __pow = function(a, b) return {value = a.value ^ b.value} end,

    -- 关系元方法
    __eq = function(a, b) return a.value == b.value end,
    __lt = function(a, b) return a.value < b.value end,
    __le = function(a, b) return a.value <= b.value end,

    -- 库定义元方法
    __index = function(t, k)
        return "You are trying to access a non-existing field: " .. k
    end,
    __newindex = function(t, k, v)
        print("You are trying to set a non-existing field: " .. k)
    end,
    __call = function(t, ...)
        local sum = 0
        for _, v in pairs({...}) do
            sum = sum + v
        end
        return sum
    end,
    __tostring = function(t)
        return "value: " .. t.value
    end,
    __len = function(t)
        local count = 0
        for _ in pairs(t) do
            count = count + 1
        end
        return count
    end,
    __pairs = function(t)
        return pairs(t)
    end,
    __metatable = function(t)
        return "This is a metatable"
    end,
    __gc = function(t)
        print("Garbage collection for table")
    end
}

-- 设置元表
setmetatable(t1, mt)
setmetatable(t2, mt)

-- 测试元方法
local t3 = t1 + t2  -- __add
print(t3.value)  -- 输出8

local t4 = t1 - t2  -- __sub
print(t4.value)  -- 输出2

local t5 = t1 * t2  -- __mul
print(t5.value)  -- 输出15

local t6 = t1 / t2  -- __div
print(t6.value)  -- 输出1.6666666666667

local t7 = t1 % t2  -- __mod
print(t7.value)  -- 输出2

local t8 = -t1  -- __unm
print(t8.value)  -- 输出-5

local t9 = t1 ^ t2  -- __pow
print(t9.value)  -- 输出125

print(t1 == t2)  -- __eq, 输出false
print(t1 < t2)  -- __lt, 输出false
print(t1 <= t2)  -- __le, 输出false

print(t1.non_existing_field)  -- __index, 输出"You are trying to access a non-existing field: non_existing_field"

t1.new_field = 10  -- __newindex, 输出"You are trying to set a non-existing field: new_field"

print(t1(1, 2, 3, 4, 5))  -- __call, 输出15

print(tostring(t1))  -- __tostring, 输出"value: 5"

print(#t1)  -- __len, 输出2

for k, v in pairs(t1) do  -- __pairs
    print(k, v)
end

print(getmetatable(t1))  -- __metatable, 输出"This is a metatable"
```

## 元表文件模块

为了方便管理和引入，可自定义元表模块

```lua


-- 文件：metatable.lua
local mt = {
    __add = function(a, b)
        return {value = a.value + b.value}
    end,
    __sub = function(a, b)
        return {value = a.value - b.value}
    end,
    __mul = function(a, b)
        return {value = a.value * b.value}
    end,
    __div = function(a, b)
        if b.value ~= 0 then
            return {value = a.value / b.value}
        else
            error("division by zero")
        end
    end,
    __tostring = function(t)
        return "value: " .. t.value
    end
}

return mt
```

```lua
-- 文件：main.lua

-- 加载元表文件
local mt = require("metatable")

-- 使用元表
local t1 = {value = 5}
local t2 = {value = 3}

setmetatable(t1, mt)
setmetatable(t2, mt)
```


## 不同元表的table操作规则


* 假设两个表 `t1`和 `t2`，它们的元表分别是 `mt1`和 `mt2`，并且 `mt1`有 `__add`元方法，`mt2`没有。那么当执行 `t1 + t2`时，Lua会使用 `mt1`的 `__add`元方法。
* 如果 `mt1`没有 `__add`元方法，但是 `mt2`有，那么Lua会使用 `mt2`的 `__add`元方法。
* 如果两个元表都有 `__add`元方法，那么Lua会使用第一个操作数的元方法。
* 如果两个元表都没有 `__add`元方法，那么Lua会报错，因为它不知道如何进行加法操作。




# 面向对象

## 使用元表和__index元方法进行类的模拟

在Lua中，虽然没有内置的类（class）概念，但是可以通过表（table）和元表（metatable）来模拟类的行为

元表作为类的结构设计，而通过index和其他函数的配合，将创建的对象（即table）的元表设为该类（该元表），从而实现面向对象的模拟

## 类的创建

```lua
-- 定义一个模拟类的表
local MyClass = {}

-- 定义类的元表
MyClass.__index = MyClass

-- 定义类的构造函数
function MyClass.new(init)
    local self = setmetatable({}, MyClass)
    self.value = init
    return self
end

-- 定义类的方法
function MyClass:add(x)
    self.value = self.value + x
end

-- 使用类
local instance = MyClass.new(5)
instance:add(3)
print(instance.value)  -- 输出8

```

## 类的继承

```lua
-- 定义基类
local BaseClass = {}
BaseClass.__index = BaseClass

function BaseClass.new(init)
    local self = setmetatable({}, BaseClass)
    self.value = init
    return self
end

function BaseClass:add(x)
    self.value = self.value + x
end

-- 定义派生类
local DerivedClass = setmetatable({}, {__index = BaseClass})

function DerivedClass.new(init, extra)
    local self = setmetatable(BaseClass.new(init), DerivedClass)
    self.extra = extra
    return self
end

function DerivedClass:multiply(x)
    self.value = self.value * x
end

-- 使用派生类
local instance = DerivedClass.new(5, 10)
instance:add(3)
instance:multiply(2)
print(instance.value)  -- 输出16
print(instance.extra)  -- 输出10

```

## ★容易混淆的核心点★

### 类模拟的底层实现

类本身是一个元表，而对象是一个table

index定义了table中查询不到元素时的行为，当table中无法找到对应方法/元素时，table会默认去index元方法定义的返回值中寻找

而在类的模拟中，index元方法默认指向定义该类的元表

所以定义元表的方法就等价于定义类的方法

其等价关系如下：

```
instance:add(3) = instance.add(instance,3) = myClass.add(instance,3)
--myClass是定义instance对象的类--
```

### 为什么有这种等价关系？

有如下两种定义方法，：是一个lua的语法糖，导致了如下的等价关系

```lua
function Person:sayHello(x)
    print(self.name)
end

function Person.sayHello(self,x)
    print(self.name)
end
```

因此在调用时可以使用如下方法调用：

```lua
bob:sayHello(1)

Person.sayHello(bob,1)
```

### 类继承的实现机制

lua中类的成员变量其实实在其元表中声明的

```lua
function BaseClass.new(init)
    local self = setmetatable({}, BaseClass)
    self.value = init
    return self
end

```

而元表通常可以实现一个new方法，作为该类的构造函数

元表本身也是table，它也可以拥有元表

因此，元表A的元表B即形成了一种继承关系，即B是A的父类

当我们试图访问A对象中的一个元素时，它会在元表A中进行查找，若元表A中未找到，则通过元表A的元表，即元表B中定义的index元方法进行构造，而元表B的index方法则指向一个元表B本身，从而形成了一种向上传递的结构

观察如下代码：

```lua
local BaseClass = {}
BaseClass.__index = BaseClass

function BaseClass.new(init)
    local self = setmetatable({}, BaseClass)
    self.value = init
    return self
end

function BaseClass:add(x)
    self.value = self.value + x
end

-- 定义派生类
local DerivedClass = setmetatable({}, {__index = BaseClass})

function DerivedClass.new(init, extra)
    local self = setmetatable(BaseClass.new(init), DerivedClass)
    self.extra = extra
    return self
end

function DerivedClass:multiply(x)
    self.value = self.value * x
end
```

基类的index指向基类本身，即元表BaseClass

派生类的new方法中，首先调用了基类的new方法进行构造，并将派生类的index方法指向基类

所以可以得出结论：

一个无需继承的类中，其元表指向自身，其index元方法也指向自身

一个派生类中，其元表指向自身，但其index方法指向一个其基类

### 值得注意的问题

lua中模拟的对象，它的封装性是基于其元表实现的，即其成员变量及成员函数本质上都是元表的对象和方法

但其本身作为一个table，仍然允许自由地插入和删除数据，这具有一定灵活性的同时，破坏了封装性

因此在lua中使用“类”，应当十分谨慎
