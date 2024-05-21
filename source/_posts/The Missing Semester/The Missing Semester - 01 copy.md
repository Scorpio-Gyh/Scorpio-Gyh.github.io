---
title: Shell工具和脚本
date: 2024-05-21 18：48：20
tags:
categories: The Missing Semester
---
# 变量声明

注意不能有空格

```
var1=bar
echo $foo
echo "value is $foo"    # value is bar
echo 'value is $foo'    # value is $foo
```

脚本示例

如果有一个mcd.sh文件，它是如下定义的：

```
mcd (){
	mkdir -p "$1"
	cd "$1"
}
```

这表示：定义名为mcd的函数，接收一个参数，创建文件夹并进入该路径

随后，在Shell中使用

```
source mcd.sh
mcd test
```

即可使用该脚本

# 各种关键字

有很多作用

$_ 上一个参数

!! 上一个命令

$? 错误代码

$@ 展开所有参数

**Shell命令支持通配符，类似正则表达式，但存在一定区别**

# 展开关键字

foo{a,b,c} = fooa foob fooc

类似的foo{a,b,c}goo{d,e}会按照笛卡尔积展开

# shellcheck shell检查

# 查找文件

ls

find

```
find .name "*.tmp" -exec rm {} \
```

找到所有tmp后缀文件并删除
