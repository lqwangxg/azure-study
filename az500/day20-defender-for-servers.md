# Day 20 - 安全中心自动化响应 与 Defender for Servers 集成

## ✅ Azure 安全中心自动化响应（Workflow automation）
- **触发器来源**：
  - 安全警报
  - 建议（Recommendations）
- **工作流操作类型**：
  - 发送邮件
  - 调用 Logic App 进行复杂响应
  - 触发 Azure Functions
- **使用场景**：
  - 安全警报一旦触发，即可调用 Logic App 自动封锁 IP、禁用用户等。

## ✅ Defender for Servers 功能
- **两种计划**：
  - **计划 1（P1）**：基础防护，如漏洞管理、基线评估。
  - **计划 2（P2）**：含 Microsoft Defender for Endpoint 集成、高级威胁检测。
- **集成能力**：
  - 可通过自动部署 Defender for Endpoint Agent（基于 Log Analytics 或 Arc）
  - 自动收集系统日志、文件完整性监控、适应性应用控制等。

## ✅ Microsoft Defender for Endpoint 与 Azure 安全中心的集成
- Defender for Endpoint 提供端点级别的行为分析与响应。
- 可与 Defender for Servers 集成，实现统一管理和威胁可视性。

## ✅ 威胁响应自动化设计建议
- 使用 Logic Apps 构建响应流程，例如：
  - 收到恶意登录告警后，自动重置用户密码。
  - 检测到恶意 IP 后，调用 NSG API 将其封锁。
