
# AZ-104 学习笔记 - Day 38

## 🔐 网络安全与远程访问核心知识

---

## 🛰️ Azure Bastion

### ✅ 特点
- **无需公网 IP** 即可远程访问 VM（RDP/SSH）
- 支持通过 Azure Portal Web 浏览器远程访问
- 默认监听端口：443（建议配合 NSG 控制）
- **不需要在 VM 上安装任何 Agent**

---

## 🔒 NSG（网络安全组）

### ✅ 核心概念
- 可应用于 **子网或网络接口卡（NIC）**
- 每条规则定义：方向（入站/出站）、端口、协议、优先级、动作
- **默认规则**：
  - 允许同一 VNet 内通信
  - 允许 Azure 平台访问
  - 拒绝所有入站/出站流量（默认规则优先级较高）

### ⚠️ 优先级说明
- 数值越小，优先级越高
- 自定义规则若优先级更高，会**覆盖默认规则**

---

## 🔥 Azure Firewall

### ✅ 功能
- **Stateful 防火墙**（与 NSG 的 stateless 不同）
- 支持应用层过滤（如按域名 FQDN）
- 支持 DNAT（入站 NAT 端口转发）
- 可与 Log Analytics 集成记录流量日志
- 与 NSG 搭配使用更安全（边界控制 + 细粒度控制）

---

## 🌩️ DDoS Protection

### ✅ 两个版本
| 类型      | 是否默认开启 | 功能特性                     | 适用场景        |
|-----------|--------------|------------------------------|-----------------|
| Basic     | ✅ 默认启用   | 基础保护（自动流量检测）     | 一般应用         |
| Standard  | ❌ 需付费启用 | 攻击缓解、告警、日志、SLA     | 企业关键系统     |

### ✅ DDoS Protection Standard
- 为指定虚拟网络启用
- 提供攻击报告、服务 SLA、Log 分析等
- 配合 Azure Firewall 使用最佳

---

## 🔗 Private Endpoint

### ✅ 特点
- 将 Azure PaaS 服务（如 Blob、SQL）映射为 VNet 内部私有 IP
- 通信 **走私网（非公网）**
- 提供更高隔离性，比 Service Endpoint 更强
- 不支持传统 NSG 控制（建议配合 Private DNS Zone 使用）
---

## 🌐 Azure、AWS、GCP 关于网络安全与远程访问的对比

| 维度               | Azure                                              | AWS                                              | GCP                                              |
|--------------------|----------------------------------------------------|--------------------------------------------------|--------------------------------------------------|
| 网络安全组         | NSG（Network Security Group）、ASG                 | Security Group                                   | Firewall Rules                                   |
| 作用范围           | 子网、NIC、ASG 动态分组                            | 实例、ENI、标签                                   | VPC、子网、实例、标签                            |
| 默认行为           | 默认拒绝入站，允许出站                             | 默认拒绝入站，允许出站                            | 默认拒绝入站，允许出站                            |
| 细粒度控制         | 支持基于 IP、端口、协议、ASG                       | 支持基于 IP、端口、协议、标签                     | 支持基于 IP、端口、协议、标签                     |
| 网络流日志         | NSG Flow Logs、Traffic Analytics                   | VPC Flow Logs、CloudWatch                        | VPC Flow Logs、Cloud Logging                      |
| 远程访问方式       | Azure Bastion（托管跳板，无公网）、JumpBox、VPN    | Systems Manager Session Manager、Bastion Host、VPN| IAP（Identity-Aware Proxy）、Bastion Host、VPN     |
| 托管跳板服务       | Azure Bastion（浏览器远程 RDP/SSH）                | Session Manager（浏览器/CLI 远程 Shell）          | IAP（浏览器/CLI 远程 Shell）、OS Login            |
| 公网暴露风险       | 可完全无公网，推荐使用 Bastion/Private Endpoint    | 可完全无公网，推荐使用 Session Manager            | 可完全无公网，推荐使用 IAP                        |
| 身份与权限控制     | RBAC、Azure AD、MFA、条件访问                      | IAM、MFA、条件策略、Cognito                       | IAM、MFA、条件策略、Identity Platform             |
| 审计与监控         | Activity Log、Monitor、Network Watcher             | CloudTrail、CloudWatch、VPC Flow Logs             | Audit Logs、Cloud Monitoring、VPC Flow Logs        |

**总结：**
- 三大云厂商均支持细粒度网络安全组、流日志和多种远程访问方式。
- Azure Bastion、AWS Session Manager、GCP IAP 均可实现无公网安全远程管理，推荐优先使用托管服务。
- 身份与权限控制、审计与监控能力均完善，建议结合安全合规和运维需求选择最佳方案。
---

## ✅ 小测验复盘（得分：13 / 20）

### 问题 1：Azure Bastion
✅ A, B, C  
❌ D（不需要安装 Agent）

### 问题 2：NSG
✅ A, C, D  
❌ B（优先级越小越优先）

### 问题 3：Azure Firewall
✅ A, B, C  
❌ D（Firewall 是 stateful，NSG 才是 stateless）

### 问题 4：DDoS Protection
✅ B, C  
❌ A（不是自动保护所有资源）  
❌ D（Standard 非默认开启）

### 问题 5：Private Endpoint
✅ A, D  
❌ B（走私网而非公网）  
❌ C（不支持用 NSG 控制）

---

## 📘 下一步预告 - Day 39 模块概览

| 模块                         | 内容                                     |
|------------------------------|------------------------------------------|
| 资源锁（Lock）               | 防止误删资源                             |
| 订阅与资源移动               | 跨订阅、资源组的移动规则                 |
| 管理组与成本中心结构         | 大型组织的多订阅治理架构                 |
| 成本分析 + Budget 设置       | 控制云支出、自动告警超预算                |
