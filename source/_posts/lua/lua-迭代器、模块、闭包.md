---
title: lua-迭代器、模块、闭包
date: 2024-06-11 15:30:41
tags: lua
---
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

`outerFunction`函数返回了一个匿名函数，这个匿名函数就是一个闭包。这个闭包可以访问 `outerFunction`的局部变量 `x`和 `y`，即使在 `outerFunction`已经返回之后。当我们调用 `closure(3)`时，它会返回 `x + y + z`的结果，即 `5 + 10 + 3`，所以输出是18。
