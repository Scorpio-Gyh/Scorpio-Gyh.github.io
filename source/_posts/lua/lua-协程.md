---
title: lua-Table
date: 2024-06-26 09:30:41
tags: lua
categories: lua
---
# 协程

协程（coroutine）是一种可以暂停和恢复执行的计算机程序组件，它提供了一种强大的编程工具，可以用来处理多任务、非阻塞I/O、状态机等复杂问题。

# 协程方法

* `coroutine.create(func)`: 创建一个新的协程。`func`是当协程被恢复时将要运行的函数。这个函数返回一个协程对象。

  ```lua
  local co = coroutine.create(function ()
      print("Hello, coroutine!")
  end)
  ```
* `coroutine.resume(co, ...)`: 开始或继续执行协程 `co`。可选的参数 `...`会被传递给协程函数或 `coroutine.yield`。如果协程运行成功，`coroutine.resume`会返回 `true`，否则返回 `false`和错误信息。

  ```lua
  local co = coroutine.create(function (a, b)
      print(a + b)
  end)
  coroutine.resume(co, 1, 2)  -- 输出：3
  ```
* `coroutine.yield(...)`: 暂停协程的执行，并返回给 `coroutine.resume`任意数量的值。

  ```lua
  local co = coroutine.create(function ()
      for i = 1, 5 do
          print(i)
          if i == 3 then coroutine.yield() end
      end
  end)
  coroutine.resume(co)  -- 输出：1 2 3
  coroutine.resume(co)  -- 输出：4 5
  ```
* `coroutine.status(co)`: 返回协程 `co`的状态。可能的返回值有"running"、"suspended"、"normal"和"dead"。

  1. "suspended"：协程已经被创建，但还没有开始运行，或者是在运行过程中调用了 `coroutine.yield`函数暂停了执行。
  2. "running"：协程正在运行。Lua不允许有多个协程同时运行，所以在任何时刻，最多只有一个协程处于"running"状态。
  3. "normal"：协程处于活动状态，但并没有在运行。它要么是因为正在等待主线程（在一个 `coroutine.resume`的调用中），要么是因为正在等待其他协程。
  4. "dead"：协程已经运行结束，或者在运行过程中发生了错误。

  ```lua
  local co = coroutine.create(function () end)
  print(coroutine.status(co))  -- 输出：suspended
  coroutine.resume(co)
  print(coroutine.status(co))  -- 输出：dead
  ```
* `coroutine.wrap(func)`: 创建一个新的协程，就像 `coroutine.create`，但返回的不是协程本身，而是一个函数。当这个函数被调用时，它会恢复协程。任何传递给这个函数的参数都会被传递给协程，作为 `coroutine.resume`的参数。这个函数返回协程的返回值，如果协程运行失败，它会抛出错误。

  ```lua
  local co = coroutine.wrap(function (a, b)
      print(a + b)
  end)
  co(1, 2)  -- 输出：3
  ```
* `coroutine.running()`: 返回当前正在运行的协程。如果在主线程中调用，返回 `nil`。

  ```lua
  local co = coroutine.create(function ()
      local running = coroutine.running()
      print(running)
  end)
  coroutine.resume(co)  -- 输出：thread: 0x7f8c8e4001d0
  print(coroutine.running())  -- 输出：nil
  ```

# 协程返回值问题

注意以下示例：

```lua
local co = coroutine.create(function (a, b)
    print("Coroutine started. Initial parameters:", a, b)
    local r = a + b
    while true do
        print("Coroutine yields:", r)
        a, b = coroutine.yield(r)
        print("Coroutine resumed. New parameters:", a, b)
        r = a + b
    end
end)

local status, result = coroutine.resume(co, 1, 2)
print("Main thread. Coroutine returned:", result)

status, result = coroutine.resume(co, 3, 4)
print("Main thread. Coroutine returned:", result)

status, result = coroutine.resume(co, 5, 6)
print("Main thread. Coroutine returned:", result)


--[[
返回值为：
Coroutine started. Initial parameters: 1 2
Coroutine yields: 3
Main thread. Coroutine returned: 3
Coroutine resumed. New parameters: 3 4
Coroutine yields: 7
Main thread. Coroutine returned: 7
Coroutine resumed. New parameters: 5 6
Coroutine yields: 11
Main thread. Coroutine returned: 11
]]--
```

`coroutine.resume`的参数（除了第一个参数，即协程对象）会成为 `coroutine.yield`的返回值，反过来，`coroutine.yield`的参数会成为 `coroutine.resume`的返回值（除了第一个布尔值的返回值）。

这种机制使得协程可以在不同的恢复点接收不同的参数，这对于实现复杂的控制流程非常有用

# 一个生成器示例

```lua
function integers()
    local i = 0
    return coroutine.create(function ()
        while true do
            i = i + 1
            coroutine.yield(i)
        end
    end)
end

local gen = integers()

for i = 1, 5 do
    print(coroutine.resume(gen))
end
```

这个生成器会生成一个无限的整数序列。每次你恢复这个生成器，它都会生成下一个整数.

每次获取整数后，可以对生成的数据进行处理。
