---
title: lua-面向对象
date: 2024-06-24 15:30:41
tags: lua

---
# lua面向对象

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
