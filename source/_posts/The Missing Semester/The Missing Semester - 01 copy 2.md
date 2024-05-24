---
title: git
date: 2024-05-24 16:20:08
tags:
categories: The Missing Semester
---
# 中文讲义：[计算机教育中缺失的一课 · the missing semester of your cs education (missing-semester-cn.github.io)](https://missing-semester-cn.github.io/)

# Git的数据结构

## 文件、文件夹模型

* 文件夹：树
* 文件：“blob”对象

## 历史记录模型

git通过有向无环图来构建数据

跟踪历史文件夹，形成文件夹快照

fork：从某个历史节点分叉，创建对象副本

merge：合并不同分支

## 模型的具体实现

```
type blob = array<byte>
type tree = map<string,tree|blob>
type commit = struct {
	parents: array<commit>
	author: string
	message:string
	snapshot:tree
}

type object = blob|tree|commit
```

每一个object通过SHA-1哈希函数存储在一个哈希表中，映射到磁盘上

git中的所有内容，都有自己的哈希值，用于寻址

git可以更新，添加，但不能更改历史内容

# 暂存区

所有的改动都会被保存在暂存区上
当提交时，可以手动add哪些内容需要提交

许多集成了Git的IDE（如VS Code）和Git GUI工具在用户选择提交时，通常会默认将所有修改过的文件添加到暂存区并一起提交。这是为了方便用户操作，避免了手动添加文件到暂存区的步骤。
