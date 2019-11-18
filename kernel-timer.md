# Linux定时器实现

一般定时器实现的方式有以下几种：

#### 基于排序链表方式：
通过排序链表来保存定时器，由于链表是排序好的，所以获取最小（最早到期）的定时器的时间复杂度为 `O(1)`。但插入需要遍历整个链表，所以时间复杂度为 `O(n)`。

#### 基于最小堆方式：
通过最小堆来保存定时器，在最小堆中获取最小定时器的时间复杂度为 `O(1)`，但插入一个定时器的时间复杂度为 `O(log n)`。

#### 基于平衡二叉树方式：
使用平衡二叉树（如红黑树）保存定时器，在平衡二叉树中获取最小定时器的时间复杂度为 `O(log n)`（也可以通过缓存最小值的方法来达到 `O(1)`），而插入一个定时器的时间复杂度为 `O(log n)`。

### 时间轮：
但对于Linux这种对定时器依赖性比较高（网络子模块的TCP协议使用了大量的定时器）的操作系统来说，以上的数据结构都是不能满足要求的。所以Linux使用了效率更高的定时器算法：__时间轮__。

__时间轮__ 类似于日常生活的时钟，如下图：

![timer](https://raw.githubusercontent.com/liexusong/linux-source-code-analyze/master/images/timer.jpg)

当时钟的秒针转一圈时，分针就会走一格，而分针走一圈时，时针就会走一格。