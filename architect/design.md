# 可选架构

目前可行方案是先基于当前市面上的开源系统做统一集成。

### 监控来源区分

**应用的不同宿主机采取相应的监控组件**

我们可以参考这种设计：

* 使用telegraf+influxdb用作虚拟机Vmware层指标监控
* 使用node-exporter + prometheus用作GuestOS层指标监控
* 使用cadvisor+prometheus用作容器层指标监控
* 使用kube-state-metrics + prometheus用作k8s层指标监控

![](https://raw.githubusercontent.com/r2ys/upic_rep/main/uPic/v2-c1940f6b811c7252722ad20bba6bd042_1440w.jpg)

### 系统整体架构设计

![](https://raw.githubusercontent.com/r2ys/upic_rep/main/uPic/archi.jpg)



监控模块主要将基础性能指标与业务服务日志进行收集和存储（图上基础指标收集与展示面板部分）。VMware层面的基础性能指标通过telegraf服务采集，上报给influxdb。GuestOS层，容器层和kubernetes 资源层性能指标分别通过node-exporter，cadvisor和kube-state-metrics来进行采集，Prometheus通过Pull的方式对数据指标进行获取。业务服务的报错通常以日志的形式进行展现，比如业务服务报错打印错误堆栈信息等，监控系统主要filebeat对业务服务的日志进行采集，并上报给logstash进行日志数据的结构化处理，最终保存到Elasticsearch中。

展示模块以grafana做为监控入口，将数据进行可视化展示，同时可以在grafana上动态设置告警规则。grafana上支持多种数据源的展示，并根据不同的数据源进行聚合查询，在面板上展示具体指标的时序图。grafana根据不同维度集成不同维度的指标面板，方便运维人员对服务运行状态的检查。grafana提供统一的告警列表面板，可展示当前告警信息与告警历史记录。

对于告警信息而言，分为业务服务告警信息和基础性能指标告警信息。业务服务告警信息对业务服务的日志有一定的要求，需要日志格式包含跟踪号Traceid，如一次http请求调用所产生的所有相关日志都应该具有同一Traceid。这样在业务服务发生错误信息告警的时候会根据Traceid链接到Kibana界面中进行相关日志信息的全量查看。对于基础性能监控指标而言，如内存使用率在深夜激增，触发告警，但是在凌晨六点钟内存恢复为正常的状态，告警解除。我们可以在告警历史记录中将这条记录找到，根据告警触发和结束时间可以连接到指定环境进行问题排查。

告警模块是由alertmanager和alertwebhook组成的，alertmanager是Prometheus监控组件中一个开源的告警管理服务，它可以对告警信息进行分组、静默和抑制等操作。alertmanager负责将告警信息转发，可以对接短信，邮箱，webhook等。alertwebhook是自研的一个告警信息管理服务，它会持久化所有告警信息，更新告警状态。