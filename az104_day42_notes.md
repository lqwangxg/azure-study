
# AZ-104 学习笔记 - Day 42

## 📊 Azure Monitor 与自动化响应

---

## 🔍 Azure Monitor 架构

### ✅ 可采集的数据类型：
| 数据类型       | 示例说明                              |
|----------------|---------------------------------------|
| 平台指标       | CPU 使用率、磁盘 IO、网络流量等        |
| 活动日志       | 资源创建、修改、删除、权限变更等事件   |
| 应用日志       | 应用内的异常、依赖调用、性能指标        |
| 自定义指标     | 通过 SDK 上报的业务指标                 |

> ❗ 注意：不支持抓包级别的原始网络流量

---

## 📁 Log Analytics 工作区

### ✅ 功能
- 存储 Azure Monitor 所采集的日志数据（结构化格式）
- 使用 **KQL（Kusto Query Language）** 进行查询
- 可连接多个资源共同发送日志数据

### ✅ 特性
- 同一订阅可建多个工作区
- 可跨 Region 与资源绑定
- 支持查询、告警、自动化等下游操作

---

## 📈 告警类型与响应机制

### ✅ 告警分类
| 类型         | 特点说明                            |
|--------------|-------------------------------------|
| 指标型告警   | 实时性高、基于系统指标              |
| 日志型告警   | 延迟略高、支持复杂多条件逻辑查询     |

### ✅ 日志告警支持
- 静态 / 动态阈值
- 多字段逻辑（如：duration > 2s 且 status == 500）
- 多种响应方式（详见 Action Group）

---

## 🚨 Action Group - 告警触发动作

### ✅ 支持的响应操作：
- 发送邮件、短信、推送通知
- 触发 Azure Automation Runbook
- 触发 Webhook（连接第三方系统）
- 调用 Function App 或 Logic App 等自动化处理

> 不支持直接“重启资源组内所有资源”，需配合脚本执行

---

## ⚙️ Azure 自动缩放（Auto-Scale）

### ✅ 功能说明：
- 基于指标（如 CPU、队列长度等）自动调整实例数量
- 支持 VMSS、App Service Plan、Functions 等

### ✅ 缩放规则
- 设置最小/最大实例数、扩容/缩容阈值
- 支持静态值或通过告警触发
- 可设置冷却时间（Cooldown）避免频繁波动
---

## 🌐 Azure、AWS、GCP 关于监控、告警与响应的对比

| 维度             | Azure                                              | AWS                                              | GCP                                              |
|------------------|----------------------------------------------------|--------------------------------------------------|--------------------------------------------------|
| 监控平台         | Azure Monitor、Log Analytics、Application Insights | CloudWatch、CloudTrail、X-Ray                    | Cloud Monitoring、Cloud Logging、Error Reporting  |
| 日志采集         | 支持多源日志采集（平台/应用/自定义）               | 支持多源日志采集（平台/应用/自定义）              | 支持多源日志采集（平台/应用/自定义）              |
| 指标监控         | 支持 VM、存储、数据库、应用等多维指标              | 支持 EC2、S3、RDS、Lambda、应用等多维指标         | 支持 VM、存储、数据库、应用等多维指标             |
| 告警配置         | 支持多条件、多维度、动态阈值、聚合分析              | 支持静态/动态阈值、组合条件、聚合分析             | 支持静态/动态阈值、组合条件、聚合分析             |
| 响应动作         | Action Group（邮件、短信、Webhook、自动化脚本、Logic App、函数等） | SNS（邮件、短信、HTTP、Lambda、自动化脚本等）     | 通知渠道（邮件、短信、Webhook、Cloud Functions等）|
| 自动化响应       | 支持与 Logic App、Automation、Functions 集成        | 支持与 Lambda、SSM Automation 集成                | 支持与 Cloud Functions、Workflows 集成            |
| 可视化分析       | Dashboard、Workbooks、KQL 分析                     | CloudWatch Dashboard、QuickSight                 | Cloud Monitoring Dashboard、BigQuery              |
| 事件管理         | 支持事件网格/Event Grid、事件驱动自动化            | 支持 EventBridge、事件驱动自动化                  | 支持 Eventarc、事件驱动自动化                     |
| SLA 与可靠性     | 高可用、全球分布、SLA 99.9%+                        | 高可用、全球分布、SLA 99.9%+                      | 高可用、全球分布、SLA 99.9%+                      |

**总结：**
- 三大云厂商均提供统一的监控、日志、告警与自动化响应平台，支持多种通知和自动化处理方式。
- Azure Monitor、AWS CloudWatch、GCP Cloud Monitoring 功能全面，均可与自动化工具（如函数、工作流）深度集成，实现智能告警响应。
- 可视化分析、事件管理和 SLA 保障能
---

## ✅ 小测验复盘（得分：14 / 20）

### 问题 1：Azure Monitor 数据源
✅ A, B, C  
❌ D：无法抓取原始封包内容

### 问题 2：Log Analytics 工作区
✅ B, C, D  
❌ A：订阅可建多个工作区

### 问题 3：日志型告警
✅ A, C, D  
❌ B：查询语言是 KQL，不是 CLI

### 问题 4：Action Group 动作
✅ A, B, C  
❌ D：不支持直接重启资源组

### 问题 5：自动缩放
✅ A, C  
❌ B：App Service 与 Functions 也支持  
❌ D：无需人工批准

---

## 📘 下一步预告 - Day 43 模块

| 模块                     | 内容说明                             |
|--------------------------|--------------------------------------|
| 网络诊断工具             | NSG Flow Log、Connection Troubleshoot |
| 网络性能监控方案         | NPM（Network Performance Monitor）    |
| Log Analytics 高级 KQL  | summarize、join、make-series 用法     |
