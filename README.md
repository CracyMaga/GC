# GC
# Unity中的GC以及优化
## Unity GC分析及优化
### 介绍
<td style="text-align: center">
  <img src="https://github.com/CracyMaga/GC/blob/main/GC.png" width="1300"/>
</td>


在游戏运行的时候，数据主要存储在内存中，当游戏的数据不在需要的时候，存储当前数据的内存就可以被回收再次使用。内存垃圾是指当前废弃数据所占用的内存，垃圾回收（GC）是指将废弃的内存重新回收再次使用的过程。Unity中将垃圾回收当作内存管理的一部分，如果游戏中垃圾回收十分复杂，则游戏的性能会受到极大影响，此时垃圾回收会成为游戏性能的一大障碍点。

### Unity内存管理机制简介
要想了解垃圾回收如何工作以及何时被触发，我们首先需要了解unity的内存管理机制。Unity主要采用自动内存管理的机制，开发时在代码中不需要详细地告诉unity如何进行内存管理，unity内部自身会进行内存管理。

unity的自动内存管理可以理解为以下几个部分：

1、unity内部有两个内存管理池：堆内存和堆栈内存。堆栈内存(stack)主要用来存储较小的和短暂的数据片段，堆内存(heap)主要用来存储较大的和存储时间较长的数据片段。
unity中的变量只会在堆栈或者堆内存上进行内存分配。

2、只要变量处于激活状态，则其占用的内存会被标记为使用状态，则该部分的内存处于被分配的状态，变量要么存储在堆栈内存上，要么处于堆内存上。

3、一旦变量不再激活，则其所占用的内存不再需要，该部分内存可以被回收到内存池中被再次使用，这样的操作就是内存回收。处于堆栈上的内存回收及其快速，处于堆上的内存并不是及时回收的，其对应的内存依然会被标记为使用状态。

垃圾回收主要是指堆上的内存分配和回收，unity中会定时对堆内存进行GC操作。

在了解了GC的过程后，下面详细了解堆内存和堆栈内存的分配和回收机制的差别。 
