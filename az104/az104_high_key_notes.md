# AZ-104 高频考点速记表（速记口诀+结构整理）

## 高频知识速记表
1. Azure Policy 用于地理/安全/合规控制部署
2. Private Endpoint 支持 File, SQL, KV, Cosmos，但不支持 VM
3. Managed Identity（System/User）用于资源授权，不用于Portal登录
4. 自动扩展需 Metric Alert（CPU等），配合 Action Group
5. Blob Contributor 支持读写权限
6. NSG 无 FQDN 支持；Firewall 支持 DNAT、日志
7. VM 可通过 Lock(CanNotDelete) 防误删
8. VM 备份通过 Recovery Services Vault 实现
9. Logic App/Runbook 更适合复杂定时任务
10. Load Balancer 为 Layer 4（TCP/UDP），AG 为 Layer 7（HTTP/S）

## ☁️ 权限控制（RBAC）
- 三层级继承：管理组 → 订阅 → 资源组 → 资源
- 经典角色：
  - Owner：完全控制
  - Contributor：创建/修改资源，无权限管理
  - Reader：只读
- Managed Identity（MI）
  - 系统分配：随资源自动创建
  - 用户分配：多个资源可共用

---

## 🌐 网络模块
- VNet = 虚拟网络，子网必须唯一 NSG 绑定
- NSG = 流量层控制（基于 IP/Port）
- Azure Firewall：
  - 支持 FQDN、DNAT/SNAT
  - 全流量日志记录
- Private Endpoint：将服务映射到私网 IP

---

## 💾 存储
- 存储账户类型：Standard / Premium，ZRS/LRS/GRS 区别
- 加密：
  - 默认：Microsoft-managed
  - 可自定义：CMK + Key Vault
- 访问方式：
  - Shared Access Signature（SAS）
  - Storage Account Key（不推荐）
  - Managed Identity（推荐）

---

## 🔍 监控与告警
- Metrics（指标）：结构化，图表形式（CPU/磁盘）
- Logs（诊断日志）：非结构化，可写 KQL 查询
- 告警方式：
  - Metrics Alert（简单阈值）
  - Log Alert + Action Group（复杂规则 + 自动动作）
- Diagnostic Setting：将日志导出到 Log Analytics / Event Hub / Storage

---

## 🛡️ 策略与治理
- Azure Policy：控制部署行为（禁止 VM SKU）
- Initiative：策略组合，统一参数传入
- Resource Lock：防止误删，级别 ReadOnly/Delete
- Tag 策略：统一打标签，供成本分析使用

---

## 💻 计算资源
- VM：IaaS，支持 Lock、Auto-Shutdown
- Function App：PaaS，可通过 MI 访问资源
- Scale Set：自动扩展 VM 实例
- App Service：支持诊断、TLS 配置、Custom Domain

---

## 📈 KQL 常见语法（用于 Log Analytics）
```kql
Heartbeat
| where TimeGenerated > ago(1h)
| summarize count() by Computer
```