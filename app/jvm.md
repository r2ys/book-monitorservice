# jvm监控

以java语言为例，关注的比较多的是jvm运行情况。



JVM 指标：

```
堆内存
heap_init：堆内存初始字节数
heap_max：堆内存最大字节数
heap_commited：堆内存提交字节数
heap_used：堆内存使用字节数

非堆内存
non_heap_init：非堆内存初始字节数
non_heap_max：非堆内存最大字节数
non_heap_commited：非堆内存提交字节数
non_heap_used：非堆内存使用字节数

直接缓冲区
direct_capacity：直接缓冲区总大小（字节）
direct_used：直接缓冲区已使用大小（字节）

内存映射缓冲区
mapped_capacity：内存映射缓冲区总大小（字节）
mapped_used：内存映射缓冲区已使用大小（字节）

GC（垃圾收集）累计详情
GcPsMarkSweepCount：垃圾收集 PS MarkSweep 数量
GcPsScavengeCount：垃圾收集 PS Scavenge 数量
GcPsMarkSweepTime：垃圾收集 PS MarkSweep 时间
GcPsScavengeTime：垃圾收集 PS Scavenge 时间

JVM 线程数
ThreadCount：线程总数量
```



