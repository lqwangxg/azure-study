
# AZ-104 学习笔记 - Day 36

## 🎯 Azure Monitor 高级功能

---

## 📌 Azure Monitor 架构概览

Azure Monitor 是 Azure 的综合监控平台，包含以下核心模块：

- **Metrics（指标）**：每分钟级别收集资源性能数据（如 CPU 使用率）
- **Logs（日志）**：诊断事件、操作日志、应用日志等，存储在 Log Analytics 工作区
- **Application Insights**：专为应用程序监控设计，支持 Live Metrics、分布式追踪等
- **Alert（告警）**：基于 Metrics 或 Logs 设置条件触发操作组
- **Autoscale（自动缩放）**：基于 Metrics 或 Schedule 规则自动调整资源实例数量

---

## 📊 Metrics 与 Logs 比较

| 特性         | Metrics                        | Logs                                     |
|--------------|--------------------------------|------------------------------------------|
| 数据类型     | 数值型时间序列（结构化）       | JSON/文本型事件数据（结构化）            |
| 采集频率     | 通常 1 分钟                    | 几分钟到十分钟不等                       |
| 延迟         | 极低（近实时）                | 相对较高（延迟 > 5 分钟）                |
| 查询语言     | 图形界面 / REST                | 使用 **KQL（Kusto Query Language）**     |
| 用途         | 告警、自动缩放、可视化仪表盘   | 审计、诊断分析、复杂查询                |

---

## 🔍 Log Analytics 工作区

- 存储 Logs 数据（VM、Function App、App Service 等输出）
- 可跨资源、订阅、区域统一查询
- 查询语言为 KQL（Kusto Query Language）

📌 示例查询：
```kql
Heartbeat
| summarize count() by Computer
```

---

## 🔧 Application Insights 特点

| 特性                       | 说明                                     |
|----------------------------|------------------------------------------|
| 自动采集请求、异常         | 内建 Agent 可接入 ASP.NET, Java, Node.js 等 |
| 分布式追踪                 | 查看请求跨服务之间的调用链               |
| 依赖跟踪                   | 检测 DB、HTTP 等外部依赖调用耗时         |
| Live Metrics 实时监控      | 查看当前请求量、失败率、服务器响应时间等 |
---

## 🌐 Azure、AWS、GCP 关于模板部署工具的对比

| 维度           | Azure                                   | AWS                                         | GCP                                         |
|----------------|-----------------------------------------|---------------------------------------------|---------------------------------------------|
| 原生模板工具   | ARM Template（JSON）、Bicep（DSL）      | CloudFormation（YAML/JSON）                 | Deployment Manager（YAML/JSON/Python）      |
| 可读性         | ARM（一般）、Bicep（高）                | YAML/JSON（较高）                           | YAML（较高）、Python（可扩展）              |
| 状态管理       | 无（需外部管理）                        | 内置堆栈状态管理                            | 内置部署状态管理                            |
| 跨云能力       | 仅 Azure                                | 仅 AWS                                      | 仅 GCP                                      |
| 第三方工具     | 支持 Terraform、Pulumi 等                | 支持 Terraform、Pulumi 等                   | 支持 Terraform、Pulumi 等                   |
| 模块化         | Bicep/ARM 支持，Bicep模块化更强          | 支持模块、嵌套模板                          | 支持模块、模板嵌套                          |
| 生态与集成     | 与 Azure CLI、Portal、DevOps 深度集成    | 与 AWS CLI、Console、CodePipeline 集成      | 与 gcloud、Console、Cloud Build 集成         |
| 资源支持广度   | Azure 全部资源                          | AWS 全部资源                               | GCP 全部资源                                |
| 语法扩展性     | Bicep 支持参数化、循环、条件等           | 支持参数化、条件、宏                        | 支持参数化、模板扩展                        |

**总结：**
- 三大云厂商均有原生模板工具，支持参数化、模块化和自动化部署。
- Azure 推荐 Bicep（可读性高），AWS 推荐 CloudFormation（YAML/JSON），GCP 推荐 Deployment Manager（YAML/Python）。
- 跨云场景建议使用 Terraform、Pulumi 等第三方工具。
- 状态管理 AWS/GCP 原生支持，Azure 需借助第三方（如 Terraform）。
- 建议结合平台生态、团队技能和自动化需求选择最佳模板工具。
---

## 🚨 告警规则与操作组

### 告警规则（Alert Rule）
- 可基于 Metrics、Logs 或 Activity Log 设定触发条件
- 可选“严重性”、“频率”、“持续时间”等配置

### 操作组（Action Group）
- 告警触发后执行的动作组
- 支持：
  - Email / SMS / Push 通知
  - Webhook
  - Azure Function
  - Logic App
  - ITSM 工单系统

---

## 🚀 Autoscale 自动缩放

支持的资源：
- App Service Plan
- VM Scale Set
- AKS 节点池（通过 KEDA/HPA）
- 自定义资源（借助 Metrics + Function）

支持两种规则：
1. **Metrics-based**：如 CPU 使用率 > 70%
2. **Schedule-based**：如每天 9:00 扩展，18:00 收缩

📌 示例：
```json
"scale": {
  "rule": "CPU > 75% for 5 min",
  "action": "increase instance count by 1"
}
```

---

## ✅ 小测验复盘（得分：9 / 10）

### 问题 1：Metrics 与 Logs 的区别
✅ B, C, D  
❌ A：Metrics 也是结构化数据

### 问题 2：Log Analytics 工作区
✅ C：统一收集资源日志并分析

### 问题 3：Application Insights
✅ A, B, C  
❌ D：支持多种语言，不仅限 .NET

### 问题 4：告警与操作组
✅ C：操作组可触发丰富自动化动作

### 问题 5：自动缩放规则
✅ A, C, D  
❌ B：App Service 也支持 Autoscale

---

## 📘 下一步预告 - Day 37 内容提要

| 模块                     | 内容                                   |
|--------------------------|----------------------------------------|
| RBAC 与策略控制          | 内置角色、条件访问、Custom Role        |
| Azure Policy             | 限制资源类型、配置一致性策略           |
| Blueprints（蓝图）       | 多资源组合的治理模板                   |
| 安全中心（Defender）     | 合规性评估、威胁防护                   |
