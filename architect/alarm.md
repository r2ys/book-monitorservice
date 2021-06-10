# 告警配置

* 告警规则设置
* 告警模型配置
* 告警问题分级以及升级管理
* 告警模版配置
* 告警发送方式接入
* 告警事件处理机制：对告警事件的处理可独立成单独的系统，支持过滤、通知、响应、处置、跟踪以及分析，实现事件全生命周期管理，最终提供事件的异常检测、根因分析、智能预测能力。



指标数据分析以及异常检测可能通过大数据和机器学习，自动识别、分级异常数据最终产生告警。



### 告警规则如何制定

一个告警规则和策略需要包含告警的统计指标、告警推送的条件、告警的时间窗口规则。

例如：

- 告警名称：网络故障告警
- 告警指标：网络速度
- 告警条件：网络速度小于20kb/s
- 统计时间：30分钟
- 发生次数：3次

表示：当某台设备30分钟内上报网速小于20kb/s大于等于3次时触发告警