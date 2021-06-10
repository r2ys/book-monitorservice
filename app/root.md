# 应用监控

#### 应用监控流程

* 应用的指标采集通常将Agent用于采集代码堆栈、资源利用、错误异常数据等，实现应用发现、应用拓扑生成、程序执行追踪等相关工作。
* 应用后端分析：应用性能管理产品接受采集的数据，对数据进行分类存储及分析处理；
* 系统管理：用户管理界面实现性能监控、故障诊断以及多维度数据统计分析。
* 实时告警

#### 应用监控功能架构

* 应用拓扑：响应请求的执行过程、调用链路
* 外部服务
* 请求分析：包括吞吐率、响应耗时、响应状态、异常、错误率、缓慢率
* 数据库分析：包括数据库操作以及响应耗时
* 代码堆栈
* 后台任务
* sql执行
* 错误&异常

##### 应用探针颁发参考图

![](https://raw.githubusercontent.com/r2ys/upic_rep/main/uPic/iShot2021-06-10%2010.37.15.png)

##### 应用健康分析参考图：

![](https://raw.githubusercontent.com/r2ys/upic_rep/main/uPic/%E5%9B%BE%E7%89%87.png)

##### 拓扑监控参考图

![](https://raw.githubusercontent.com/r2ys/upic_rep/main/uPic/iShot2021-06-10%2014.13.26.png)

##### 请求分析参考图

![](https://raw.githubusercontent.com/r2ys/upic_rep/main/uPic/req1.png)

![](https://raw.githubusercontent.com/r2ys/upic_rep/main/uPic/req2.png)

##### 请求堆栈分析

![](https://raw.githubusercontent.com/r2ys/upic_rep/main/uPic/iShot2021-06-10%2010.32.26.png)

