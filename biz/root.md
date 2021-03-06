# 业务监控

## 背景

业务系统类似飞机，我们应该关注服务及业务的运行情况，而且通常来说，后台开发负责的是在快速飞行的飞机上修零件；边飞边升级。所以就像飞机仪表盘时刻指示着各个模块的运行情况，我们业务系统都应该有自己的仪表盘-**`业务监控告警`**。

## 目标

我们希望通过**业务监控告警**保证我们的业务和服务的可靠性，最终达到如下的效果：

* 我们要能够及时的发现问题（先于客户、运营发现问题）。
* 不想每天半夜爬起来处理不知道怎么来的问题（有一些事情准备）。
* 出了问题，能够帮助我们，快速定位问题。

## 问题点

### 关注点

站在业务角度，一个业务服务经常关注的点有如下：

* 机器系统资源\(CPU/内存/网络/磁盘\)、CDN等
* JVM虚拟机运行情况
* 接口的调用量、耗时情况、错误异常
* 具体业务（如注册、开户、交易等流程）
* 具体运营

#### 相关指标梳理参考

![](https://raw.githubusercontent.com/r2ys/upic_rep/main/uPic/iShot2021-06-10%2011.41.41.png)

### 如何从监控数据定义异常

通常来讲，每天同一时间的监控数值应当一致。基于这个依据，我们可以做一些比较以发现问题。

* **同比**：当前时间跟昨天同一时刻进行比较，是上升还是下降。 用于突然的掉底或上升问题发现。
* **环比**：当前一段时间与前一段时间比较，一般用得比较少。
* **七日平均值**：当前时间与前七天同一时刻平均值进行同比。减少某天波动带来的影响。

### 异常发现时间

综合成本以及时效性的权衡，一大部分采用实时监控，部分需要综合比较监控数据的情况采用隔日处理

* 实时监控处理：强调实时性，针对关键节点、存在少量数据不一致或丢失情况。
* 隔日处理：有充足的资源、全量节点、数据准确、利用大数据能力。

### 如何在代码中自定义异常

通常的方式是定义切入点，埋入不影响业务运行的代码片，异步输出。

* 通过接口层、业务逻辑层、数据库层、包括下游渠道层做埋点，手动捕捉异常并输出。
* 针对其他关注点执行特殊异常处理，最终输出。

### 异常程度

异常分级处理：

* **忽略**：一些可以忽略的数据波动，影响比较小的、不紧急的情况。这类可忽略的异常通常是某些告警异常的前奏。
* **通知**：提醒负责人该关注一下这里的问题，视情况而处理，比如账户余额、交易量上升等。这部分是需要权衡的，如果一些比较关注的点发出通知，比如可接受的交易成功率出现比较大的下滑、或某些交易接口请求量上升的时候就可以人工介入。
* **告警**：出现比较大的问题了，需要安排人力紧急排查了。

### 通知

跟上面异常程度相对应，有不同的告警方式通知关注者。

紧急程度依次是：数据图表 &lt; 微信 &lt; 短信/电话

* **数据图表**：对应**忽略**、**通知**
* **微信**：对应**通知**
* **短信/电话**：对应**告警**

如何做到既减少骚扰又能及时发现问题：

1. 做到高内聚低耦合，暴露给外部是最简单的
2. 精细化处理某些点，根据场景采取不同策略

## 收到告警后的处理

很多时候收到通知或告警后难定位问题，这个时候凸显出监控数据图表的重要性，可结合图表分析问题。

### 如何处理

#### 1 业务初期的简单处理

业务或系统的初期，没有人力去做，但监控告警是必要的，此时可以确认关键点，根据数据库记录，做统计和分析。

* 定时扫描订单数据库统计同比；
* 根据确定波动阈值告警

#### 2 业务平稳后的计算统计

针对全量扫表的性能问题，需要将监控告警独立成单独的模块。

* 将小粒度的实时统计数据保存到统计数据库；
* 跑批对比分析同比数据；

#### 3 日志收集分析系统

利用公用的监控告警系统，将业务关注的点通过抽象行为日志上报给代理日志中心，大数据中心做数据分析和存储。

* 定义需要统计的节点的维度、度量、原始数据留存时间。
* 业务系统上报日志或输出日志
* 代理日志中心将日志数据临时存储、再同步到大数据中心。
* 大数据中心根据节点、统计维度、度量、统计时间做交叉数据统计。
* 根据交叉数据做实时监控告警。
* 根据明细数据做全量数据报表。

### 业务监控功能需求概况

有别于硬件、应用监控，业务监控应当从业务语义指标去衡量应用健康度，能直观地体现例如当日下单平均响应时间、成功率等业务问题。意味着从业务视角衡量应用性能和稳定性，对业务的关键交易进行全链路的监控。业务监控通过追踪并采集应用程序中的业务信息，实时展现业务级的指标，例如业务的响应时长、次数和错误率，最终将应用程序和业务表现之间做映射关联。 

#### 业务监控的实现方式

理想的方式是应用探针：

| 监控方式                 | 接入成本                                                     | 实时性                                     | 灵活性                                             |
| ------------------------ | ------------------------------------------------------------ | ------------------------------------------ | -------------------------------------------------- |
| **业务监控（应用探针）** | **低（业务信息在应用程序中自动采集上报）**                   | **实时（后台实时聚合运算展现）**           | **高（灵活配置业务映射规则，立即生效）**           |
| 自定义监控（日志）       | 高（需要改造应用程序，在日志中打印业务信息）                 | 实时                                       | 低（新增的分析需求需要更改日志后才能展示业务信息） |
| 传统OLAP BI分析          | 高（为避免影响在线业务处理性能，需要新建离线分析数据库，定期同步数据） | 非实时（由于数据同步的间隔，无法实时分析） | 中（取决于同步的业务数据是否齐全）                 |

#### 可视化业务规则配置

**以无侵入方式可视化定义业务请求**

通常在HTTP请求的Header、 请求参数和Session中，或者在RPC调用的请求参数中都包含有业务信息，例如订单金额、用户名称、用户属性、业务动作和来源等。 业务监控通过Java Agent的方式，实时采集这些业务信息，连带相应的URL和接口名等信息一同上报。

可以在业务监控的控制台，通过可视化界面灵活地定义某个业务信息与URL、RPC接口的映射关系，包括需要匹配的信息和拆分的维度， 完成业务与服务调用的关联。

参考阿里云ARMS的规则配置能力：

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5957197951/p111631.png)



#### 贴合业务的性能指标与诊断能力

业务监控默认提供某一业务的应用链路拓扑，以及吞吐量、响应时间和错误率等指标，同时可以关联到相应的数据库请求、异常和各级调用链路。

参考阿里云ARMS的业务监控指标诊断能力：
![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6957197951/p111634.png)



