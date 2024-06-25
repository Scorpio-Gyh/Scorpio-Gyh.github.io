---
title: lua-元表和元方法
date: 2024-06-15 15:30:41
tags: lua
---
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
