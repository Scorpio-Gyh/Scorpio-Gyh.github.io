---
title: C++基础
date: 2024-05-27 15:30:41
tags: C++
---
# VS相关

## 解决方案

# 工作流程

```cpp
#include <iostream>

int main(){
	std::cout<<"Hello World"<<endl;
	std::cin.get();
}
```

预处理指令："#"，发生在编译之前

编译：将文本文件转换为机器码，.cpp文件会被编译，但.h文件不会，.h文件会在预处理阶段被“copy”到.cpp文件中，编译的结果是一个object文件，在windows系统中，是.obj文件

链接：将所有obj缝合，构建依赖关系，并形成exe文件
