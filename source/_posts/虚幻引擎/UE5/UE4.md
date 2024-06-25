---
title: Unreal Engine 5 C++
date: 2024-05-13 10:48:15
categories:
- 虚幻引擎
tags:
- 游戏引擎
---
# UE的宏

UPROPERTY	标识某个属性托管于UE，可以修改，它提供了一系列参数，用于定义属性和行为

* `EditAnywhere`：属性可以在任何地方进行编辑。
* `BlueprintReadOnly`：属性在蓝图中只读，但在C++中可写。
* `BlueprintReadWrite`：属性在蓝图和C++中都可读写。
* `VisibleAnywhere`：属性在任何地方都可见，但不能编辑。
* `VisibleDefaultsOnly`：属性只在默认属性面板中可见。
* `VisibleInstanceOnly`：属性只在实例属性面板中可见。
* `EditDefaultsOnly`：属性只能在默认属性面板中编辑。
* `EditInstanceOnly`：属性只能在实例属性面板中编辑。
* `Category`：定义属性在编辑器中的分类。
* `Transient`：属性不会被序列化和保存。
* `BlueprintAssignable`：属性可以被蓝图赋值。
* `BlueprintCallable`：函数可以在蓝图中被调用。
* `BlueprintNativeEvent`：函数可以在蓝图中被覆盖，但也可以在C++中调用。

UFUNCTION 标识函数托管于UE，可进行操作

* `BlueprintCallable`：函数可以在蓝图中被调用。
* `BlueprintImplementableEvent`：函数可以在蓝图中被覆盖，但在C++中没有默认实现。
* `BlueprintNativeEvent`：函数可以在蓝图中被覆盖，但也可以在C++中调用。
* `BlueprintPure`：函数在蓝图中是一个纯函数，即它不会改变游戏世界的状态。
* `Exec`：函数可以被控制台命令调用。
* `Server`，`Client`，`NetMulticast`：函数可以在网络游戏中被远程调用。
* `Event`：函数是一个事件，可以被其他函数调用。

# UE打印

* UE_LOG：输出控制台
* GEngine：AddOnScreenDebugMessage 打印在运行屏幕
