
# AZ-104 学习笔记 - Day 7

## 🎯 学习目标
- 掌握 Azure Monitor 架构与数据来源
- 使用 Log Analytics 和 Application Insights 诊断问题
- 理解指标告警与日志告警的区别与配置方式
- 熟悉 AutoScale 自动缩放机制
- 理解 ARM 模板的声明式部署特性
- 测验验证监控与自动化相关知识

---

## 🧠 Azure Monitor 架构总览（逻辑结构）

Azure Monitor 是 Azure 的统一监控平台，由以下模块组成：

```
资源（VM / Function / App / DB）
    ↓
监控数据（指标 / 日志 / 活动日志）
    ↓
数据平台（Azure Monitor）
    ├─ Log Analytics（结构化日志查询）
    ├─ Application Insights（应用性能追踪）
    ├─ Metrics Explorer（指标可视化）
    ↓
告警（Alerts） + 动作组（Action Group）
    ↓
响应（邮件 / SMS / Webhook / Azure Function / Logic App）
```

---

## 🔍 Log Analytics & KQL 基础

```kql
# 检索过去 1 小时的异常
AppExceptions
| where TimeGenerated > ago(1h)
| summarize count() by problemId
```

> 常用表：
- `AppRequests`：应用调用信息
- `AppExceptions`：错误与异常
- `Heartbeat`：VM 运行状况
- `AzureDiagnostics`：平台资源日志

---

## 📈 Application Insights

| 功能 | 描述 |
|------|------|
| 应用级性能监控 | 请求延迟、依赖失败 |
| 自动异常收集 | 错误堆栈追踪、分布式调用链 |
| SDK 集成 | 支持 .NET, Node.js, Java 等 |
| 查询语言 | 同样使用 KQL |

---

## 🚨 告警机制详解：指标告警 vs 日志告警

| 项目 | 指标告警（Metric Alert） | 日志告警（Log Alert） |
|------|---------------------------|------------------------|
| 数据来源 | 实时数值 | KQL 查询结果 |
| 延迟 | 近实时（1-5 分钟） | 通常 >5 分钟 |
| 示例 | CPU > 80%，请求数 > 1000 | 错误数 > 10，某类异常出现 |
| 灵活性 | 简单阈值判断 | 可复杂逻辑处理 |
| 配置界面 | Portal 可视化配置 | 需手写查询语句 |

---

## 📦 动作组（Action Group）

可将告警触发时连接多个通知或响应：

- Email / SMS / 电话
- Webhook / Logic App
- Azure Function / Automation

---

## 🔄 AutoScale 自动缩放配置示例

> 目标：VM Scale Set 在 CPU 超过 70% 时增加实例数

```json
{
  "autoscaleSettings": {
    "targetResourceUri": "/subscriptions/<sub>/resourceGroups/<rg>/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS",
    "profiles": [
      {
        "name": "cpu-scale-out",
        "capacity": {
          "minimum": "2",
          "maximum": "10",
          "default": "2"
        },
        "rules": [
          {
            "metricTrigger": {
              "metricName": "Percentage CPU",
              "metricNamespace": "",
              "operator": "GreaterThan",
              "threshold": 70,
              "statistic": "Average",
              "timeAggregation": "Average",
              "timeGrain": "PT1M",
              "timeWindow": "PT5M"
            },
            "scaleAction": {
              "direction": "Increase",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          }
        ]
      }
    ]
  }
}
```

---

## 🧰 ARM 模板（Azure Resource Manager）

| 特点 | 描述 |
|------|------|
| 格式 | JSON（非 YAML） |
| 类型 | 声明式、参数化 |
| 支持功能 | 模块化部署、资源间依赖管理 |
| 用途 | 实现基础设施即代码（IaC） |

✅ 推荐结合 Bicep 或 Terraform 做模块化、可读性更强的配置管理。

---

## 🧪 Day 7 测验回顾（5/5 满分 🎉）

| 题号 | 正确答案 | 说明 |
|------|-----------|------|
| 1 | C | Application Insights 专用于请求响应与依赖性能监控 |
| 2 | B | Activity Log 记录控制平面操作 |
| 3 | A, C, D | Metric Alert 配合 Action Group |
| 4 | C | `summarize count()` 用于聚合计数 |
| 5 | B, C, D | ARM 模板支持参数化与批量部署，不是 YAML |

---

## 📌 总结记忆重点

- Azure Monitor = [指标 + 日志 + 活动日志] 的集中分析平台
- Application Insights = 应用视角的性能与诊断工具
- Metric Alert 快速简单，Log Alert 灵活强大
- AutoScale 配置需配合 Metrics/Alert
- ARM 模板实现声明式基础设施部署（JSON 格式）

---

## 📅 下一步 - Day 8 预告

| 模块 | 内容 |
|------|------|
| 备份与恢复 | Azure Backup / Recovery Vault |
| 快照 | VM 磁盘快照 / Blob 版本控制 |
| 还原策略 | 文件级还原 / 整体 VM 恢复 |
| 小测验 | 备份保留期、快照与恢复策略判断 |
