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

```
function f (a,b)
	print(a,b)
end
```

下列调用均合法，若多于定义，则舍弃，若少于定义，则以nil补充

```
f()
f(0)
f(0,0)
f(0,0,0)
```

## 可变参数

```
function f(...)
	local a,b,c,d = ...
	print (a,b,c,d)
```

## 多返回值

```
function f(a,b)
	return a,b
end

m,n = f(3,5)
```

## 函数为参数

直接传递即可

```
function sum(a,b)
	return a+b
end

function f(m,n,fun)
	local res = fun(m,n)
	print(res)
end
```

可以传递匿名函数

```
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

```
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

```
while a > 0 do
	print(a)
end
```

# repeat..until

```
repeat
	print(a)
	a = a - 1
until a <= 0
```

# for循环

意为for(int i = 10;i <= 50 ;)

```
for i = 10, 50 ,20 do
	print(i)
end
```

# break 同C++

# goto

实现循环

```
function f(a)
	::flag:: print("------")
	if(a > 0) then
		print(a)
		a = a - 1
		goto flag
	end
end
```
