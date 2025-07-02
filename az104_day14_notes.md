# AZ-104 学习笔记 - Day 14

## 🎯 今日主题：Azure Bastion 与访问控制架构

---

## 🧱 Azure Bastion 服务

| 特性           | 描述                                           |
|----------------|------------------------------------------------|
| 无公网访问     | 可通过浏览器访问 VM，无需分配公有 IP          |
| 协议支持       | RDP (3389)、SSH (22)                          |
| 安全性         | 连接通过 TLS 加密，不暴露 IP 地址            |
| 子网要求       | 部署在名为 `AzureBastionSubnet` 的子网中，最小 /26 |
| 高可用性       | 自动跨可用区部署（如支持）                    |

> ✅ 不需要 VM 配置公网 IP，也无需开放 NSG 中的 RDP/SSH 端口。

---

## 🧱 JumpBox（跳板机）与 Azure Bastion 对比

| 项目          | JumpBox                          | Azure Bastion                       |
|---------------|----------------------------------|-------------------------------------|
| 部署方式      | 独立 VM                           | 托管服务                            |
| 公网访问      | 需要公网 IP 和 NSG                | 不需要                              |
| 安全性        | 易误配置、需补丁更新              | 更安全，免维护                      |
| 成本          | VM 成本 + 管理成本                | 按小时计费，自动高可用              |
| 管理负担      | 需手动打补丁、备份                | 完全托管                            |

---

## 🔐 管理子网设计（Management Subnet）

- 用途：隔离远程管理路径。
- 常见组件：JumpBox、Azure Bastion、VPN Gateway、Log Analytics 私有连接等。
- 建议：
  - 绑定 NSG，只允许可信 IP 或 ASG。
  - 可结合 UDR，强制出站流量经防火墙。

---

## ✅ 实践建议（访问架构）

| 目标             | 建议方案                                 |
|------------------|------------------------------------------|
| 隐藏公网 IP      | 禁用所有生产 VM 的 Public IP              |
| 安全远程管理     | 使用 Azure Bastion                        |
| 管理网络隔离     | 设置 `mgmt-subnet` 并绑定严格 NSG          |
| 强制流量路径     | 使用 UDR 导向 Azure Firewall / NVA       |
| 审计与追踪       | 启用 Log Analytics + Activity Log + RBAC |

---
---

## 🌐 Azure Bastion 与 AWS 相关资源对比

| 功能/场景         | Azure 实现                          | AWS 实现                                 | 主要区别说明                                   |
|-------------------|-------------------------------------|------------------------------------------|------------------------------------------------|
| 托管跳板服务      | Azure Bastion                       | AWS Systems Manager Session Manager      | AWS SSM 支持浏览器远程管理，无需公网/跳板机    |
| 传统跳板机        | JumpBox（自建 VM）                  | Bastion Host（自建 EC2）                 | 均需自行维护、配置安全组/NSG                   |
| 远程协议支持      | RDP、SSH（浏览器/门户）             | SSH、RDP（SSM 支持 CLI/浏览器/门户）     | AWS SSM 支持更多自动化和无代理访问              |
| 公网暴露风险      | Bastion/JumpBox 可完全无公网         | SSM 可完全无公网，Bastion Host需公网IP   | 托管服务均可消除公网暴露                       |
| 子网要求          | 专用 AzureBastionSubnet，/26         | 无特殊要求，建议单独子网                  | Azure 有命名和大小要求，AWS 灵活                |
| 安全组/NSG        | 建议绑定 NSG，限制访问               | 建议绑定 Security Group，限制访问         | 控制方式类似                                   |
| 审计与追踪        | Log Analytics + Activity Log + RBAC  | CloudTrail + CloudWatch + IAM            | 均支持细粒度审计和权限管理                     |
| 访问方式          | 浏览器直接访问 Azure 门户            | AWS 控制台/SSM/CLI/浏览器                | AWS SSM 支持命令行和自动化                     |

**总结：**
- Azure Bastion 和 AWS SSM Session Manager 都可实现无公网安全远程管理，推荐优先使用托管服务。
- 传统 JumpBox/Bastion Host 在两大云平台均需自行维护，安全风险和运维成本更高。
- 子网、访问控制、审计等机制两者均支持，命名和集成方式略有差异，建议结合实际场景选择
---

## ✅ 小测验回顾（5 / 5）

### 1. 使用 Bastion 的前提条件？
- ✅ B. 需部署 `AzureBastionSubnet`，最小 /26
- ✅ D. 用户需有 VM 登录权限（RBAC）
- ❌ A. 不需要 VM 绑定公网 IP
- ❌ C. VM 不需要单独配置 NSG 放通 RDP/SSH

---

### 2. JumpBox 的缺点？
- ✅ A. 增加公网暴露风险
- ✅ B. 需自行维护/更新补丁
- ✅ D. 错误配置可能导致安全漏洞
- ❌ C. JumpBox 是可以结合 NSG 使用的

---

### 3. 限制 VM 仅能通过 Bastion 访问？
- ✅ A. 禁用 VM 公网 IP
- ✅ B. 配 NSG 限制仅 Bastion 子网访问
- ✅ C. 配置 UDR 强制流量走向受控路径
- ❌ D. 无需删除 AzureBastionSubnet 的 NSG

---

### 4. Azure Bastion vs JumpBox 最大区别？
- ✅ A. 是否通过浏览器连接 VM

---

### 5. AzureBastionSubnet 的配置要求？
- ✅ A. 子网名称必须为 `AzureBastionSubnet`
- ✅ B. 子网大小至少 /26
- ✅ D. 子网中不能部署其他资源
- ❌ C. NSG 可选，但不是必须条件

---

## 🧠 今日重点记忆

- **Azure Bastion** 是官方托管的远程访问服务，安全替代 JumpBox。
- 无需公网 IP，无需手动管理更新，默认使用浏览器安全访问。
- 管理子网（mgmt-subnet） + NSG + UDR 是推荐的网络安全架构。
- 结合 Log Analytics、RBAC 可实现安全审计与访问追踪。

---

## 📅 下一步预告 - Day 15

| 模块                     | 内容                             |
|--------------------------|----------------------------------|
| Azure DNS                | 公网和私网名称解析机制            |
| Private DNS Zone         | 专用 DNS 区域，绑定 VNet 使用     |
| DNS 与 VNet 集成原理     | 如何实现自动解析与隔离控制        |
