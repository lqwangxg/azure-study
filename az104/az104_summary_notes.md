# AZ-104 思维导图结构：

```pgsql
1. 身份与权限管理
   ├─ Azure AD 基础概念：Tenant / Object / Group
   ├─ RBAC 权限管理：Owner / Contributor / Reader
   ├─ Managed Identity：System / User-assigned
   └─ 多重身份认证、条件访问策略

2. 网络配置
   ├─ VNet / Subnet 构建
   ├─ NSG：规则绑定限制（子网或 NIC）
   ├─ Load Balancer / Application Gateway 区别
   ├─ Azure Firewall（DNAT/SNAT/FQDN 过滤）
   └─ Private Endpoint vs Service Endpoint

3. 计算资源管理
   ├─ VM 创建 / 备份 / Auto-shutdown / Lock
   ├─ VM Scale Set 与自动扩展设置
   ├─ Function App / Web App / AKS 概览
   └─ 计算资源监控指标：CPU、磁盘、网络

4. 存储与数据库
   ├─ 存储账户类型：Standard / Premium
   ├─ 加密机制：Platform-managed key / CMK
   ├─ Blob/SAS/Private Endpoint/Firewall 限制
   └─ Azure SQL / CosmosDB 的连接方式

5. 监控与告警
   ├─ Azure Monitor 架构图
   ├─ Metrics vs Logs 区别
   ├─ Diagnostic Settings / Activity Logs
   ├─ Log-based 告警 + Action Group
   └─ Application Insights 性能监控

6. 策略与治理
   ├─ Azure Policy vs RBAC 区别
   ├─ Initiative / 参数化策略管理
   ├─ Azure Blueprints 概述
   └─ Resource Lock、标签策略、合规性检查
```

## 🩺 Diagnostic Settings 使用场景详解

**Diagnostic Settings（诊断设置）** 用于将 Azure 资源的监控数据（如活动日志、资源日志、指标）导出到指定的目标（如 Log Analytics 工作区、Storage Account、Event Hub），便于长期存储、集中分析、合规审计和自动化响应。常见使用场景包括：

- **合规与审计**：企业需长期保存资源操作和安全日志，满足合规要求（如等保、ISO）。
- **集中日志分析**：将多资源的诊断日志汇总到 Log Analytics，便于统一查询、关联分析和故障排查。
- **自动化告警与响应**：日志流向 Event Hub，触发外部系统或自动化流程（如 SIEM、告警平台）。
- **成本优化**：将高频日志导出到 Storage Account，降低长期存储成本。

### 📋 示例：为存储账户配置 Diagnostic Settings

**需求**：将某存储账户的所有操作日志和指标导出到 Log Analytics 工作区，便于安全审计和集中监控。

**操作步骤**：
1. 在 Azure Portal 打开目标存储账户，选择“诊断设置”。
2. 新建诊断设置，勾选所需的日志类别（如“读取”、“写入”、“删除”操作日志）和指标。
3. 选择“发送到 Log Analytics 工作区”，并指定目标工作区。
4. 保存设置。

**效果**：
- 所有存储账户的操作和性能数据自动汇聚到 Log Analytics。
- 支持 KQL 查询、可视化分析、自动化告警和合规审计。
- 日志数据可长期保存，满足安全与合规

## 🌀 Event Hub 使用场景详解

**Event Hub** 是 Azure 的高吞吐量数据流平台，适用于实时数据采集、日志聚合、流式分析等场景。它支持将大量事件数据从各种源（如应用、设备、服务）高效地收集并传递到下游分析、存储或处理系统。

### 典型使用场景

- **实时日志与遥测采集**：收集应用、IoT 设备、基础设施的日志和遥测数据，供后续分析和监控。
- **大数据管道入口**：作为大数据平台（如 Azure Stream Analytics、Databricks、HDInsight）的数据入口，实现实时 ETL 和分析。
- **跨系统事件集成**：在微服务、分布式系统中，用于解耦生产者和消费者，实现高可用、可扩展的事件驱动架构。
- **安全与合规日志归集**：配合 Diagnostic Settings，将资源日志流向 Event Hub，供 SIEM 或第三方安全平台消费。

### 📋 示例：将 Azure 诊断日志发送到 Event Hub 供 SIEM 消费

**需求**：企业希望将所有关键资源的操作日志实时推送到第三方安全平台（如 Splunk、QRadar）进行安全分析。

**操作步骤**：
1. 在 Azure Portal 创建 Event Hub 命名空间和 Event Hub 实例。
2. 在目标资源（如存储账户、VM、网络安全组）配置 Diagnostic Settings，选择“发送到 Event Hub”并指定目标 Event Hub。
3. 在第三方安全平台或自建消费端，配置 Event Hub 连接器，实时拉取和处理日志数据。

**效果**：
- 所有资源的操作日志实时汇聚到 Event Hub。
- 第三方平台可实时消费日志，实现安全监控、威胁检测和合规分析。
- 支持高吞吐、弹性扩展，适合大规模企业级日志归集

---

## 🌐 Azure Event Hub、AWS Kinesis、GCP Pub/Sub 对比

| 维度             | Azure Event Hub                        | AWS Kinesis（Data Streams/Firehose）         | GCP Pub/Sub                                 |
|------------------|----------------------------------------|----------------------------------------------|---------------------------------------------|
| 服务类型         | 实时事件流平台，支持高吞吐、批量消费   | 实时数据流（Streams）、批量投递（Firehose）  | 实时消息发布/订阅，事件驱动                 |
| 消息模型         | 分区日志（Partitioned Log）            | 分片日志（Shards）                           | 发布-订阅（Pub/Sub）                        |
| 消费方式         | 拉取（Pull）、捕获（Capture）           | 拉取（Pull）、Firehose自动投递               | 推送（Push）和拉取（Pull）                  |
| 保留时间         | 1~90天（标准），最长7天（基本）         | 24小时~7天（Streams），Firehose自动投递      | 默认7天，可扩展                             |
| 单分区吞吐       | 1MB/s 入站，2MB/s 出站（标准）          | 1MB/s 入站，2MB/s 出站/分片                  | 无分区限制，自动扩展                        |
| 自动扩展         | 需手动调整分区数（标准），高级版支持自动扩展 | 需手动调整分片数，Firehose自动扩展           | 自动扩展，无需手动分区                      |
| 集成分析         | 支持 Stream Analytics、Databricks等     | 支持 Kinesis Analytics、Redshift、S3等       | 支持 Dataflow、BigQuery、DataProc等          |
| 事件捕获         | 支持自动捕获到 Blob/ADLS                | Firehose 自动投递到 S3、Redshift等           | 需自定义 Dataflow 或 Sink                   |
| 消息顺序保证     | 分区内有序                              | 分片内有序                                   | 支持有序投递（订阅级别）                    |
| 安全与权限       | RBAC、托管身份、VNet、加密              | IAM、KMS、VPC、加密                          | IAM、KMS、VPC、加密                         |
| 典型场景         | 日志采集、IoT、实时分析、SIEM           | 日志采集、IoT、实时分析、数据湖              | 日志采集、IoT、实时分析、事件驱动架构        |

**主要差异总结：**
- Azure Event Hub 和 AWS Kinesis 都以分区/分片为核心，GCP Pub/Sub 自动扩展、无分区限制，弹性更强。
- GCP Pub/Sub 支持推送和拉取，Event Hub/Kinesis 以拉取为主。
- Firehose/事件捕获可自动投递到存储，便于大数据分析。
- 权限、加密、集成分析能力三者均完善，建议结合业务规模、弹性需求和生态选择。

**SIEM**（Security Information and Event Management，安全信息与事件管理）是一类用于集中收集、存储、分析和关联企业 IT 系统中的安全日志和事件的安全平台。SIEM 主要功能包括：

- **日志收集与归档**：从服务器、网络设备、应用、云服务等收集安全日志和事件。
- **实时监控与告警**：对日志进行实时分析，发现异常行为并自动告警。
- **关联分析**：将不同来源的事件进行关联，识别复杂攻击链和威胁。
- **合规与审计**：长期保存日志，生成合规报告，满足法规要求（如等保、ISO、GDPR）。
- **取证与溯源**：支持安全事件发生后的调查和溯源分析。

常见 SIEM 产品有 Splunk、IBM QRadar、Microsoft Sentinel（Azure）、ArcSight 等。
---