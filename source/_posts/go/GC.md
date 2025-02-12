---
title: GC
tags: [Learn, Go]
category: [Learn, Go]
cover: /img/Go_Logo.png
description: GC
data: 2025-02-12
---
# Go Tri-color Marking Algorithm and Hybrid Write Barrier Mechanism

## Principles of the Tri-color Marking Algorithm

The tri-color marking algorithm implements concurrent garbage collection marking through color states (white, gray, black):

1. **Root Set Scanning**: Starting from root objects (stacks, global variables, etc.), mark initially accessed nodes as gray
2. **Gray Object Processing**: Change gray nodes to black and mark their downstream unvisited nodes as gray
3. **Iterative Processing**: Repeat gray node processing until no gray nodes remain
4. **Sweep Phase**: Reclaim all white nodes

Color semantics:
- White: Unscanned objects (potential garbage)
- Gray: Discovered but incompletely scanned objects
- Black: Confirmed live and fully scanned objects

## Object Loss Problem

Living objects may be erroneously reclaimed when both conditions are met:
1. **Black References White**: A black object directly references a white object
2. **Gray Loses Reachability**: All paths from gray objects to the white object are severed

Example scenario:
Black A → White B (Condition 1)
Gray C ⇢ White B reference deleted (Condition 2)

White B loses protection and will be incorrectly reclaimed.

## Barrier Techniques

### Strong/Weak Tri-color Invariants
- Strong tri-color invariant: Prohibits black objects from directly referencing white objects
- Weak tri-color invariant: Allows black-to-white references but requires the white object to be protected by other gray objects

### Insertion Barrier
- Rule: When a black object inserts a reference to a white object, mark the white object as gray
- Characteristics:
  - Requires STW stack rescan after marking completes

### Deletion Barrier
- Rule: When deleting an object reference, mark the deleted reference target as gray
- Characteristics:
  - Lower reclamation accuracy (retains more floating garbage)
  - Requires initial STW scan of live objects at GC start

## Go 1.8 Hybrid Write Barrier

### Core Rules
1. **Initial Marking**: Concurrently scan all Goroutine stacks at GC start, marking stack objects as black
2. **New Objects**: Directly mark new objects created on stacks during GC as black
3. **Reference Deletion**: Mark deleted reference targets as gray
4. **Reference Addition**: Mark added reference targets as gray

> [!NOTE]
> 
> Barrier techniques are not applied to stack operations

### Implementation Characteristics
- Stack space disables write barriers: Rules 1 & 2 ensure stacks remain black
- Satisfies weak tri-color invariant

### Advantages
1. **Low STW Overhead**:
   - Initial stack scanning executes concurrently without pausing
   - Final stack rescanning becomes unnecessary
2. **High Precision**:
   - Combines advantages of insertion/deletion barriers

## Technical Evolution Comparison

| Feature     | Insertion Barrier | Deletion Barrier | Hybrid Write Barrier |
|-------------|-------------------|-------------------|----------------------|
| STW Phase   | Final stack scan | Initial heap/stack scan | None          |
| Precision   | High              | Low                | High                |

# Go 三色标记法与混合写屏障机制

## 三色标记法原理

三色标记法通过颜色状态（白、灰、黑）实现垃圾回收的并发标记：

1. **Root Set 扫描**：从根对象（栈、全局变量等）出发，将首次访问的节点标记为灰色
2. **灰色处理**：将灰色节点变为黑色，并将其下游未访问节点标记为灰色
3. **循环执行**：重复处理灰色节点直到没有灰色存在
4. **清理阶段**：回收所有白色节点

颜色语义：
- 白色：未被扫描的对象（潜在垃圾）
- 灰色：已发现但未完成扫描的对象
- 黑色：已确认存活且完成扫描的对象

## 对象丢失问题

当同时满足以下两个条件时会导致存活对象被误回收：

1. **黑色引用白色**：黑色对象直接指向白色对象
2. **灰色丢失可达**：灰色对象到白色对象的所有路径被切断

示例场景：

黑A → 白B（条件1）
灰C ⇢ 白B 的引用被删除（条件2）

此时白B失去保护，将被错误回收。

## 屏障技术

### 强/弱三色不变式
- 强三色不变式：禁止黑色对象直接引用白色对象
- 弱三色不变式：允许黑色引用白色，但该白色必须被其他灰色对象保护

### 插入屏障
- 规则：当黑色对象插入指向白色对象的引用时，将白色对象标记为灰色
- 特点：
  - 标记结束后需 STW 重新扫描栈对象

### 删除屏障
- 规则：当删除对象引用时，将被删除对象标记为灰色
- 特点：
  - 回收精度较低（保留更多浮动垃圾）
  - GC 开始时需要 STW 扫描初始存活对象

## Go 1.8 混合写屏障

### 核心规则
1. **初始标记**：GC 开始时并发扫描各 Goroutine 栈，将栈上对象全部标记为黑色
2. **新建对象**：GC 期间栈上创建的新对象直接标记为黑色
3. **引用删除**：被删除的对象标记为灰色
4. **引用添加**：被添加的对象标记为灰色

> [!NOTE]
>
> 屏障技术不应用于栈

### 实现特点
- 栈空间不启用写屏障：通过规则1、2保证栈始终为黑色
- 满足弱三色不变式

### 优势分析
1. **低 STW**：
   - 初始栈扫描并发执行，无需暂停程序
   - 最终无需重新扫描栈空间
2. **高精度**：
   - 结合插入/删除屏障优点

## 技术演进对比
|   特性   | 插入屏障   | 删除屏障     | 混合写屏障 |
| :------: | ---------- | ------------ | ---------- |
| STW 阶段 | 最终栈扫描 | 初始堆栈扫描 | 无         |
| 回收精度 | 高         | 低           | 较高       |
