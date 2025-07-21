
# Azure Monitor：Log Analytics vs Application Insights

## ✅ 一句话区分

| 名称 | 一句话解释 |
|------|------------|
| **Log Analytics** | 一个 **通用的日志查询和存储平台**（用 KQL 分析所有遥测数据） |
| **Application Insights** | 一个 **专注于应用性能和使用分析的服务**，用于 APM（Application Performance Monitoring） |

---

## 🧩 架构关系图（重点）

```
                             +-------------------------+
                             |  Application Insights   | ← 应用程序遥测（APM）
                             +-------------------------+
                                          ↓
                            【收集】应用日志、请求、异常、依赖 等
                                          ↓
               +-----------------------------------------------+
               |    Log Analytics Workspace（日志分析平台）     |
               +-----------------------------------------------+
                  ↑        ↑        ↑
              VM Agent  AMA/DCR  Container
              系统日志   网络日志   docker 输出
```

> ✅ **Application Insights 的数据，其实就是存储在 Log Analytics Workspace 中！**

---

## 📦 关键区别表

| 项目 | Application Insights | Log Analytics |
|------|----------------------|---------------|
| **主要用途** | 应用性能监控（APM） | 日志分析平台 |
| **采集对象** | 应用程序（Function App、Web App、API） | 所有资源（VM、容器、网络、存储等） |
| **支持语言** | .NET, Java, Node.js, Python 等 SDK | 不区分语言，只看数据源 |
| **查询语言** | KQL（Kusto） | KQL（Kusto） |
| **日志表名** | `requests`, `traces`, `exceptions`, `dependencies` | `Syslog`, `Heartbeat`, `CustomLogs_CL`, `Perf` 等 |
| **是否必须依赖 Workspace** | 可独立使用，也可关联 Log Analytics | 必须依赖 Workspace |
| **告警功能** | 内建（Smart Detection、异常识别） | 可自定义查询告警 |
| **收费模式** | 按数据量、保留天数 | 同上，共享计费 |

---

## 🧠 更通俗理解

| 场景 | 应该用 |
|------|--------|
| 我想分析 Function App 的执行时间、异常堆栈、请求量 | ✅ Application Insights |
| 我想分析 VM 容器日志、系统日志、安全日志 | ✅ Log Analytics |
| 我希望统一收集和查询所有日志（Function + VM + Storage） | ✅ Log Analytics Workspace（统一平台） |
| 我只关心 Web API 的性能表现 | Application Insights 足够 |
| 我想用 KQL 查询所有资源产生的日志 | Log Analytics 是主平台（App Insights 的数据也在里面） |

---

## 📌 示例：Function App 使用 Application Insights 的日志表

| 表名 | 含义 |
|------|------|
| `requests` | 每个 HTTP 调用（URL、响应码、耗时） |
| `dependencies` | 外部依赖（API、DB）调用信息 |
| `exceptions` | 错误堆栈 |
| `traces` | 应用内部日志（console.log / logger） |
| `customEvents` | 你自定义的事件 |

---

## 🧪 示例：KQL 查询应用日志（来自 App Insights）

```kql
requests
| where timestamp > ago(1h)
| where success == false
| project name, url, resultCode, duration
```

---

## ✅ 总结

| 结论 | 描述 |
|------|------|
| Log Analytics 是底层“数据平台” | 它存储来自 App Insights、VM、Storage 等的日志 |
| Application Insights 是“应用监控服务” | 它收集应用层的遥测信息，分析性能、异常、依赖等 |
| 推荐做法 | 把 Application Insights 和 Log Analytics Workspace **关联起来统一查询** |
