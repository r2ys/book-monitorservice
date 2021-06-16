# Introduction

## 企业应用统一监控告警方案

### 前言

本文从以下出发点入手，旨在针对所有业务系统、中台服务、基础服务等制定统一的监控告警标准。

* [监控维度](metrics/root.md)
* [业务监控](biz/root.md)
* [链路监控](trace/root.md)
* [应用监控](app/root.md)
* [容器和主机监控](hardware/root.md)
* [统一监控告警系统设计](architect/root.md)



### 需求调研

**如何建设生产环境下的系统可观测性能力**

监控报警是业务系统的金钟罩，对业务系统非常重要。不存在不出问题、没有 Bug 的系统。系统上线之后，就免   不了出现故障，出故障是或早或晚的事，是或多或少的事。我们能做的是，出现故障之后，第一时间知道，赶紧处理，防止影响扩大。再理想点，如果故障出现之前，刚有苗头的时候，我们就能发现，提前解决就更好了。业务系统的监控报警，就是干这个用的。

通常来说一个没有监控的系统，在运行期间对问题的反馈通常是这个流程：

线上bug反馈：用户-客服-产品负责人/项目负责人-开发

**如何做到快速发现bug/如何做到出现之前就发现**

举一个简单的业务调用的例子：

三方响应时间预期是2秒，程序设置超时时间是5s，通过对响应时间进行监控，在接近5秒时就主动预警，通过联系下游渠道解决，就杜绝了超时之后告警的问题。



### 目标

监控和告警的目标则是一致的，即：

- 全面：监控与告警的广度，尽可能多的覆盖到故障类型
- 及时：数据处理和传递的时效性，快速的将告警信息暴露出来
- 准确：保证监控和告警信息的准确性，避免浪费相应的资源去解决错误的告警。



### 附录

#### 参考文档：

[基于Telegraf Influxdb Grafana的监控平台介绍](http://www.360doc.com/content/19/1011/20/54737980_866206874.shtml)

[Telegraf开源组件](https://github.com/influxdata/telegraf)

[cAdvisor InfluxDB Grafana搭建监控平台](http://book.akhack.com/swarm/_book/swarm_book/%E7%BE%A4%E9%9B%86%E7%9B%91%E6%8E%A7.html)

[基于Telegraf的数据收集系统](https://cloud.tencent.com/developer/news/377267)

[使用Prometheus+Grafana快速打造高逼格监控平台](https://zhuanlan.zhihu.com/p/75188816)

[Telegraf 使用](https://blog.csdn.net/youngtong/article/details/84640382)

[指标与日志的监控告警系统设计](https://zhuanlan.zhihu.com/p/345562326)

[一篇文章了解监控告警](https://zhuanlan.zhihu.com/p/60416209)

[如何专业化监测一个 Kubernetes 集群？](https://mp.weixin.qq.com/s?__biz=MzU4NzU0MDIzOQ==&mid=2247495067&idx=2&sn=8274481d8f0304663ad9a407526831ae&chksm=fde8dffbca9f56ed50eb6eb0c8e8c3a75aeaaa02f9918689e33a764207541bc0c698fbabc365&mpshare=1&scene=1&srcid=0531DNFCaBDdBzThyZZ2Gsex&sharer_sharetime=1622469135139&sharer_shareid=cbc82124cedc2b0864b983af90cce39a#rd)

[接口链路统计](https://www.cnblogs.com/huane/p/6025498.html)

[阿里云ARMS链路监控](https://zhuanlan.zhihu.com/p/321792372)

[浅谈全链路监控](https://www.cnblogs.com/imyalost/p/10941216.html)

[Prometheus学习](https://mp.weixin.qq.com/s?__biz=MzUzMTA2NTU2Ng==&mid=2247517826&idx=2&sn=a14d699d30132942c322ff6ac9d3cb0d&chksm=fa4af933cd3d70258d15b63699b5d8ce1c9cbf0c2b462ce4b753e211ed2be20647fb0873e7f8&scene=0&xtrack=1#rd)

[阿里云应用实时监控服务ARMS产品文档](https://helpcdn.aliyun.com/product/34364.html)

[Micrometer 快速入门](https://www.cnblogs.com/cjsblog/p/11556029.html)

[Spring Boot 使用 Micrometer](https://cloud.tencent.com/developer/article/1508319)

[Prometheus 监控埋点](https://www.dazhuanlan.com/2019/12/07/5deb3278abd56/)

[Prometheus](https://prometheus.io/download/)

[Prometheus中文文档](https://prometheus.fuckcloudnative.io/di-san-zhang-prometheus/di-2-jie-an-zhuang/gettingstarted)