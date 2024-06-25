---
title: lua-Table
date: 2024-06-07 15:30:41
tags: lua
categories: lua
---
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
