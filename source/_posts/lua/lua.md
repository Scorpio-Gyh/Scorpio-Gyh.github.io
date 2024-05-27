---
title: lua
date: 2024-05-27 15:30:41
tags:游戏引擎
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

and

break

do 

else

elseif

end

false

for

function

if

in

local

nil

not

or

repeat

return

then

true

until

while

goto

除此之外，Lua定义了很多内置全局变量以 _ + 大写字母构成，如_ABC

# 变量

Lua为弱类型语言，变量无需声明

Lua变量为动态类型，可随时改变类型

变量默认为全局变量

局部变量以 local 声明
