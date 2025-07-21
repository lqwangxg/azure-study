# AZ-104 学习笔记 - Day 48

## 📊 Azure Resource Graph（ARG） 与 Kusto Query Language（KQL）

---

### 📘 Azure Resource Graph 概要

| 特性            | 说明                                                                 |
|-----------------|----------------------------------------------------------------------|
| 查询方式        | 基于 KQL（Kusto Query Language）语言                                |
| 使用权限        | 读权限即可使用（Reader）                                             |
| 使用方式        | Portal → Resource Graph Explorer / Azure CLI / PowerShell            |
| 跨订阅支持      | ✅ 可跨订阅、跨管理组进行资源查找                                     |
| 修改资源        | ❌ 不支持修改，仅限查询                                               |

---

### 🧠 常见应用场景

- 查询未加标签（如 costCenter）的资源  
- 查询使用某种 SKU 的 VM  
- 查询所有资源所在 region 分布  
- 审计是否符合合规策略（如是否加密、是否启用日志）

---

## 🔎 常用 KQL 语法

```kql
resources
| where type == "Microsoft.Compute/virtualMachines"
| project name, location, resourceGroup
| summarize count() by location
```

- where: 类似 SQL 的 WHERE，用于条件过滤
- project: 类似 SQL 的 SELECT，指定输出字段
- summarize: 类似 SQL 的 GROUP BY，用于聚合统计
- distinct: 去重

### 🧪 小测验复盘（得分：10 / 17）
问题 1：Resource Graph 特性
- ✅ B, C
- ❌ A：ARG 不创建资源
- ❌ D：KQL ≠ Azure Policy JSON

问题 2：VM 查询语句
- ✅ resources | where type == "Microsoft.Compute/virtualMachines"

问题 3：KQL 操作符
- ✅ A, B, D
- ❌ C：KQL 不用 SELECT，用 project

问题 4：可实现功能
- ✅ A, B
- ❌ C：不能修改资源
- ✅ D（漏选）：可查询 SQL Server 的 SKU

问题 5：ARG 优势
- ✅ A, B
- ❌ C：不可写
- ✅ D：可用于自定义资源审计报告（漏选）

### 📌 小结建议
- Resource Graph 是资源治理与审计的强力工具
- 学会基本 KQL 是今后做资源报告和合规查询的基础
- 推荐练习题目：查询所有未加标签的资源、按 Region 汇总 VM 数量

### 🔜 下一模块预告（Day 49）
|内容 | 	说明|
|--- | --- |
|综合复盘：网络 / 权限 / 存储	|全模块错题回顾，重点内容再串讲|
|模拟考试准备	| 结合测评系统（MeasureUp / Whizlabs）|