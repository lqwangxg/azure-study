# 📘 AZ-500 Day 18 - 安全警报与事件响应

## 🎯 核心知识点

### 1. 🔎 Microsoft Defender for Cloud 警报（Security Alerts）
- **来源：**
  - Azure 资源（如 VM、AKS、SQL、App Services）安全异常
  - 来自 Defender for Endpoint、Defender for Identity 等产品的整合警报
- **重要字段：**
  - Severity（严重等级）
  - Resource（受影响资源）
  - Time generated（触发时间）
  - MITRE ATT&CK 映射（攻击链对照）

---

### 2. 🧠 安全建议（Security Recommendations）
- 与警报不同，建议用于**识别潜在安全弱点**
- 来源于 **Azure Policy + Defender for Cloud 基线规则**
- 可导出为 CSV 或与 Power BI/Logic Apps 结合处理

---

### 3. ⚙️ 安全自动化（Security Automation）
- 可通过 **Logic App + 安全中心触发器** 实现：
  - 自动发送邮件/通知
  - 自动隔离 VM
  - 自动创建工单或更新 CMDB
- 示例流程：
  1. 警报触发 → 2. Logic App 收到 → 3. 条件判断严重性 → 4. 通知/隔离

---

### 4. 🔄 跨平台日志收集与 SIEM 集成
- **Log Analytics / Azure Monitor** 可将日志传送至 SIEM 平台
  - 如：Splunk, QRadar, Sentinel
- **Azure Event Hub** 可作为中转站将数据转发给外部系统

---

### 5. 🔐 安全事件响应流程（Incident Response Lifecycle）
常见步骤：
| 阶段       | 描述 |
|------------|------|
| Preparation | 建立 IR 策略、训练人员、配置工具 |
| Detection   | 利用 Defender/Sentinel 检测威胁 |
| Containment | 临时隔离受影响资源（如 NSG 限制、VM 断网） |
| Eradication | 根除攻击者访问（更新凭据、修补漏洞） |
| Recovery    | 恢复业务正常运行 |
| Lessons Learned | 事后复盘、优化防御措施 |

---

### 6. 🛡️ Sentinel 中的响应机制
- 支持**自动响应规则（Automation Rules）**和**Playbooks（Logic Apps）**
- 支持多种来源触发：
  - 规则匹配的警报
  - 异常行为分析（UEBA）
  - 自定义 KQL 查询触发

---

## ✅ 工具 & 服务对比

| 工具 | 用途 | 触发能力 | 自动化集成 |
|------|------|-----------|------------|
| Defender for Cloud | 风险检测 + 建议 | 是 | 支持 Logic App |
| Azure Monitor / Log Analytics | 日志收集 & 分析 | 部分 | 强 |
| Azure Sentinel | SIEM & SOAR 平台 | 强 | 强 |
| Logic Apps | 自动化流程执行 | 强 | 原生 |

---

## 🆚 Defender for Cloud vs Sentinel 功能与区别
| 功能/属性  | Microsoft Defender for Cloud     | Microsoft Sentinel                   |
| ------ | -------------------------------- | ------------------------------------ |
| 类型     | CSPM + CWPP（安全姿态+工作负载保护）         | SIEM + SOAR（事件管理 + 自动响应）             |
| 主要用途   | 安全建议、合规性评估、威胁检测、工作负载防护           | 安全日志分析、威胁检测、可视化、自动化响应                |
| 数据来源   | Azure 资源、AWS、GCP、本地 Agent        | 多平台日志（Azure、AWS、GCP、Firewalls、等）     |
| 可视化界面  | 安全评分、建议、合规报告                     | 威胁图、工作簿（KQL 可视化）、分析仪表板               |
| 自动响应   | 通过 Defender 触发 Logic App         | 使用 Automation Rules + Playbooks 自动响应 |
| 定价模式   | 按启用资源和保护范围收费                     | 按日志吞吐量（GB）和存储时间收费                    |
| 深度防护支持 | 虚拟机、容器、Key Vault、SQL、Kubernetes等 | 无直接防护，专注于日志分析与响应                     |
| 使用门槛   | 中等，主要基于平台服务                      | 稍高，需要 KQL、SIEM 知识                    |
| 集成程度   | 与 Azure 平台深度整合                   | 可扩展性强，适合复杂多源环境                       |
