[TOC]



# 框架选型

监控告警系统的流程主要分为四大部分，目前通用性强的开源方案由以下四部分组成：

* 采集
* 存储
* 可视化
* 日志监控




### 采集

#### [Telegraf](https://github.com/influxdata/telegraf)

InfluxData公司提供了一整套包括采集、存储、展示、监控告警的解决方案：TICK STACK（Telegraf、InfluxDB、Chronograf、Kapacitor）。核心采集组件是Telegraf，一个用于收集和发送指标的插件驱动的服务器代理，内存占用小，可以用来定时获取VMware vCenter的虚拟机资源数据。Telegraf拥有插件和集成功能，用于收集系统和服务的各项指标(主要是时间序列数据，比如CPU、MEM)。支持输入输出插件。通过插件系统开发人员可轻松添加支持其他服务的扩展。

输入端可以从以下来源获取指标数据：

* 直接获取操作系统的指标
* 从三方api获取指标
* 通过StatsD、kafka获取指标

输出端可以发送指标数据给以下数据存储：

* 数据存储db：InfluxDB、OpenTSDB、Graphite、Datadog、Librato、Kafka、MQTT、NSQ
* 服务
* 消息队列

常用的输入插件mysql、redis、prometheus等都有。

**原理**

定时执行输入插件收集数据->执行处理插件和聚合插件->批量输出到存储

![](https://raw.githubusercontent.com/r2ys/upic_rep/main/uPic/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_9e1db1e6-08b8-405e-90ce-5fca91855c1e.png)



#### cAdvisor

Google的监控工具。需要在所有节点安装，用于收集监测单节点容器资源信息。

#### Prometheus

Go语言开发。Prometheus是一套开源的监控&报警&时间序列数据库的组合。

**原理**

通过HTTP协议周期性抓取被监控组件的状态，任意组件只要提供对应的HTTP接口就可以接入监控，是比较适合 Docker，Kubernetes 等环境的监控系统之一。输出监控信息的HTTP接口被称作 **exporter**。可以采用Pull或者Push的方式获取监控指标数据。建议使用的是 `Pull` 模型，使用Pull方式对监控指标数据从采集服务点(Agent)进行拉取。下图中Prometheus Server 通过 HTTP 的 pull 方式到各个目标拉取监控数据。exporter可以是Redis、Nginx、Node

![](https://raw.githubusercontent.com/r2ys/upic_rep/main/uPic/v2-98f7fcc9807e0f23d4a1464f71afe0cf_1440w.jpg)



- **Retrieval**：中定义了 Prometheus Server 需要从哪些地方拉取数据

- - **Jobs / Exporters**：Prometheus 可以从 Jobs 或 Exporters 中拉取监控数据。Exporter 以 Web API 的形式对外暴露数据采集接口。
        - **Prometheus Server**：Prometheus 还可以从其他的 Prometheus Server 中拉取数据。
        - **Pushgateway**：对于一些以临时性 Job 运行的组件，Prometheus 可能还没有来得及从中 pull 监控数据的情况下，这些 Job 已经结束了，Job 运行时可以在运行时将监控数据推送到 Pushgateway 中，Prometheus 从 Pushgateway 中拉取数据，防止监控数据丢失。
        - **Service**：是指 Prometheus 可以动态的发现一些服务，拉取数据进行监控，如从DNS，Kubernetes，Consul 中发现。

- **Storage**：即 Prometheus 的存储，利用 Prometheus Server 的本地存储。

- **PromQL**：是 Prometheus 的查询语句，利用 PromQL 可以和一些 WEBUI (如 Grafana )集成

- **AlertManager**：是一个独立于 Prometheus 的外部组件，用于监控系统的告警，通过配置文件可以配置一些告警规则，Prometheus 会把告警推送到 AlertManager。



### 存储(时序数据库)

#### InfluxDB

TICK STACK中的开源组件。Go语言开发，无依赖。是一个分布式时序、事件和指标数据库,用于处理高写入和查询负载。可以用来收集容器监控数据

#### OpenTSDB

#### Graphite

#### TimescaleDB



### 可视化组件

#### Grafana

监控绘图工具，纯Javascript 开发的前端工具，流行度高。通过多种数据源(如InfluxDB、Prometheus等），作为监控告警的入口，展示自定义报表、显示图表。同Kibana类似，大多使用在时序数据的监控方面。支持的时序数据库有：InfluxDB、OpenTSDB、Graphite、Prometheus、Elasticsearch；支持关系数据库MySQL、PostgreSQL。Go语言开发。自带用户、告警功能。通过配置db数据源展示数据。可以实时收集、存储、显示时间序列类型的数据。

#### Chronograf

[Chronograf](https://docs.influxdata.com/chronograf/v1.5/)是TICK STACK中的展示组件Web应用程序。将Chronograf与TICK堆栈的其他组件一起使用，可以显示监控数据并创建警报和自动化规则。



### 日志监控

#### felk

filebeat(日志采集)、elasticsearch(分布式的存储引擎)、logstash(日志数据做结构化处理)和kibana(监控面板)。市面上成熟的日志监控方案

![](https://raw.githubusercontent.com/r2ys/upic_rep/main/uPic/v2-b7af563322b1d87b0a74ecc393fe2694_1440w.jpg)

es的特点：提供了丰富的查询和聚合功能。可以对索引进行生命周期管理，如超过时间范围内的数据进行冷热部署，删除等操作。