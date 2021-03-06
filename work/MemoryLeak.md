# 内存泄漏（Memory Leak）

问题描述：

- 最近在工作中遇到一个 Memory Leak 的问题，内存还是一直被占用，手动 GC 也无法处理。

解决方案：

1. 在 Collection 或 Map 使用完成后，设置为 null：因为这样才能释放存入其中的对象

2. 释放各种连接

3. 单例模式在初始化后始终在 JVM 的生命周期中存在，如果其持有了外部的引用，则该引用的对象无法被回收

4. 使用完成的对象，赋值为 null

5. 使用 `Runtime.getRuntime().gc();`，也就是 `System.gc();` 触发 GC

6. 如果是线程一直无法释放，尝试结束线程

衍生问题：

- 内存碎片: 如果 GC 使用的是 Mark Sweep，可能会存在大量内存碎片。这就需要使用其他 GC 方式
