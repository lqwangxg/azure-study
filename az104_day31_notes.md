
# AZ-104 学习笔记 - Day 31

## 📊 App Service 的诊断、监控与故障排查

---

## 🧾 应用日志与诊断工具

### 🔹 App Service 日志类别

| 日志类型                     | 描述 |
|------------------------------|------|
| Application Logging（应用日志） | 记录 stdout/stderr 输出 |
| HTTP Logs（Web 服务器日志）     | 记录每次请求的 HTTP 状态、时间等 |
| Detailed Errors（详细错误日志） | 捕捉 4xx/5xx 页面详细错误 |
| Failed Request Tracing（失败请求跟踪） | 用于分析请求失败的详细信息（主要针对 IIS） |

### 🔸 Kudu 工具（https://<app>.scm.azurewebsites.net）

- 查看文件系统、环境变量
- 进程监控
- Git 部署记录
- Web 控制台调试

### 🔸 Diagnose and Solve Problems

- Portal 内建智能诊断助手
- 快速定位启动失败、响应慢、连接中断等问题

---

## 📈 应用性能监控与自动扩展

### 🔹 Application Insights（APM 工具）

| 功能               | 描述 |
|--------------------|------|
| 请求和依赖追踪     | HTTP 请求和数据库调用可视化 |
| Smart Detection    | 自动侦测异常行为（异常率飙升等） |
| Live Metrics       | 实时请求、失败、响应时间 |
| 自定义事件和指标   | 应用内手动埋点 |
| 性能分析           | 响应耗时、瓶颈分析 |

支持语言：.NET, Java, Node.js, Python

---

### 🔹 自动扩展（Auto Scale）

- 仅支持水平扩展（添加/移除实例）
- 可基于指标如 CPU、内存、HTTP 请求、App Insights 指标等配置规则

| 示例规则                     | 行为 |
|------------------------------|------|
| CPU > 70% 持续 10 分钟         | 实例数 +1 |
| HTTP 请求 > 1000 / min       | 实例数 +1 |
| App Insights 自定义指标触发   | 实例数变化 |

---

## ⚙️ 故障排查与恢复机制

### 🔸 Restart Policy

- App Service 实例故障自动重启
- 平台负责，不支持自定义脚本

### 🔸 Health Check（运行状况探测）

- 设置 `/healthz` 路径供平台探测
- 多次失败将从负载中移除实例
---

## 🌐 Azure、AWS、GCP 关于诊断、监控与故障排查的对比

| 维度           | Azure                                      | AWS                                         | GCP                                         |
|----------------|--------------------------------------------|---------------------------------------------|---------------------------------------------|
| 应用日志       | App Service 日志（应用、HTTP、错误、跟踪） | CloudWatch Logs（应用、访问、错误日志）     | Cloud Logging（应用、请求、错误日志）       |
| 诊断工具       | Kudu、Diagnose and Solve Problems           | CloudWatch Logs Insights、X-Ray、CloudShell | Cloud Debugger、Error Reporting、Cloud Shell|
| 性能监控/APM   | Application Insights（APM、分布式追踪）     | X-Ray（APM）、CloudWatch Metrics            | Cloud Trace、Cloud Profiler、Cloud Monitoring|
| 自动扩展       | Auto Scale（基于指标自动水平扩展）          | Auto Scaling（基于指标自动扩展）            | App Engine/Cloud Run 自动扩缩容             |
| 健康检查       | Health Check（自定义探测路径）              | Elastic Load Balancer Health Check          | Health Checks（负载均衡/实例级）            |
| 故障恢复       | 实例自动重启、健康探测摘除                  | 实例自动恢复、健康探测摘除                  | 实例自动重启、健康探测摘除                  |
| 监控平台       | Azure Monitor、Log Analytics                | CloudWatch、CloudTrail                      | Cloud Monitoring、Cloud Logging             |
| 告警与自动化   | Monitor 告警、Action Group、Logic App       | CloudWatch Alarm、SNS、Lambda               | Alerting Policy、Cloud Functions            |
| 可视化分析     | Dashboard、Workbooks、Traffic Analytics     | CloudWatch Dashboard、QuickSight            | Cloud Monitoring Dashboard、BigQuery         |

**总结：**
- 三大云厂商均提供全面的日志采集、性能监控、APM、自动扩展、健康检查和故障恢复能力。
- Azure Application Insights、AWS X-Ray、GCP Trace/Profiler 均支持分布式追踪和性能分析。
---

## ✅ 小测验回顾（4 / 5）

### 1. 哪个日志记录 stdout/stderr 输出？
- ✅ B. Application Logging（应用日志）

### 2. 哪个工具用于远程调试、命令执行？
- ✅ C. Kudu 工具

### 3. Application Insights 功能？
- ✅ A. 实时日志采集  
- ✅ B. 请求/依赖追踪  
- ✅ D. 性能分析和异常监控  
- ❌ C. 配置 VNet 隔离功能与其无关

### 4. 关于 Auto Scale 正确描述？
- ✅ B. 支持 HTTP 请求数触发扩展  
- ✅ C. 仅支持水平扩展 ❗（你漏选）

### 5. Health Check 功能？
- ✅ A. 探测健康状况并自动摘除实例

---

## 📅 下一步预告 - Day 32

| 模块 | 内容 |
|------|------|
| 应用配置与 Key Vault 整合 | 密钥注入 / 配置自动化 |
| App Service 与混合连接 | Hybrid VNet Integration / Private DNS |
| 高可用部署策略 | 多区域部署 + Traffic Manager |
