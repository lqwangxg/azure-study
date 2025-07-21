
# AZ-104 学习笔记 - Day 2

## 🎯 学习目标
- 理解 Azure 虚拟网络（VNet）与子网结构
- 掌握 NSG（网络安全组）的控制机制
- 区分 Service Endpoint 与 Private Endpoint 的使用场景
- 理解网络安全的精细化控制方式

---

## 🌐 虚拟网络（Virtual Network）

- VNet 是 Azure 中的私有网络，提供 IP 地址段（CIDR 形式）、子网划分、资源隔离和连接。
- 每个 VNet **只能存在于一个 Region（区域）**，不能跨区域。
- 子网（Subnet）是 VNet 中的逻辑划分。

示例结构：

```
VNet: 10.2.0.0/16 (Region: Japan East)
├── Subnet: frontend (10.2.1.0/24)
└── Subnet: backend  (10.2.2.0/24)
```

---

## 🛡️ 网络安全组（Network Security Group, NSG）

NSG 是一种虚拟防火墙，用于控制入站和出站流量，支持规则级别的优先级与协议、端口、源/目标地址控制。

- 每条规则包括：优先级（数值越小优先）、协议、端口、源/目标、允许/拒绝。
- 可关联对象：
  - ✅ 子网（Subnet）
  - ✅ 虚拟机的网络接口（NIC）
  - ❌ Function App 本身（不可直接，但可通过集成子网控制出站）

⚠️ 限制：
- 每个子网只能关联 **一个 NSG**（这是平台设计限制，不只是 Portal 限制）。

---

## 🔄 Service Endpoint vs. Private Endpoint

| 项目 | Service Endpoint | Private Endpoint |
|------|------------------|------------------|
| 工作方式 | 通过 VNet 强化访问控制 | 分配私网 IP 到 VNet 子网 |
| 控制流量 | 出站增强控制 | 入站 & 出站全私有 |
| 安全性 | 中等 | 高（强 DNS 控制 + 私有通信） |
| 示例服务 | Storage, SQL,Key Vault  等 | Storage, Key Vault, Web App 等 |

### 🔐 Service Endpoint 如何控制网络流量
通过将虚拟网络（VNet）的子网与 Azure PaaS 服务（如存储、SQL、Key Vault 等）紧密绑定，实现以下方式来控制网络流量。
  1. 专用 Azure 骨干网络： 开启服务终结点后，流量会通过 Azure 的内部骨干网络，从子网的私有 IP 发向服务，避免使用公共 Internet 地址
  2. 源 IP 变更为私有地址： 服务端看到的源 IP 是 VNet 子网私有 IP，而不是 VM 的公共 IP，这样你可以通过 Azure PaaS 服务的防火墙规则精确限制来源
  3. 网络安全组（NSG）中的 Service Tag 控制： 可在 NSG 出站规则中使用如 Microsoft.Storage，Microsoft.Sql 这类 Service Tag，将子网仅允许向已启用服务终结点的服务流量发送，其他流量仍受控制
  4. 服务终结点策略（Endpoint Policies）：对某些服务（如 Azure Storage）可以设置更细粒度的访问策略，允许子网只能访问特定的存储账户或资源组

### ✅ Service Endpoint 当前支持服务列表
以下是 Azure 官方支持通过 Service Endpoint 绑定并控制流量的服务（“Generally available”）
- Azure Storage（Microsoft.Storage）
- Azure Storage 跨区域服务终结点（Microsoft.Storage.Global）
- Azure SQL Database（Microsoft.Sql）
- Azure Key Vault（Microsoft.KeyVault）
- Azure Service Bus（Microsoft.ServiceBus）需 Premium 级别 
- Azure Event Hubs（Microsoft.EventHub）

除此之外，通过 Service Endpoint 控制的服务可包括（但不限于）：
- Azure Data Lake Gen1（通过 Entra ID claims）
- Microsoft.Web（适用于 App Service 和 Azure Functions，配合 VNet 集成使用）

### 📋 Service Endpoint 总结表
| 功能       | 描述                                      |
|----------|-----------------------------------------|
| Security | 源 IP 限定为子网私有 IP，支持 NSG Service Tag 控制流量 |
| 专线通道     | 流量走 Azure 内部网络，不经公共 Internet            |
| 终结点策略    | 限制子网只能访问特定资源（如某存储账户）                    |
| 绑定方式     | 子网启用服务终结点 + 在目标服务上设置 VNet 规则            |

---

## 🚦 Azure Function App 与 NSG

- Azure Function App 本身 **不能直接绑定 NSG**
- 若使用 **区域 VNet 集成（Regional VNet Integration）**，可以通过所绑定子网的 NSG 控制其**出站流量**
- **入站访问流量（HTTP 调用）始终通过 Azure 平台控制，无法由 NSG 限制**

---

## 🛠️ 实操任务回顾

1. 创建 VNet: `vnet-day2`（10.2.0.0/16）
2. 子网：
   - frontend: 10.2.1.0/24
   - backend: 10.2.2.0/24
3. 创建 NSG 并添加规则：
   - 允许 TCP 80（HTTP）流量来自 Internet
   - 拒绝 TCP 22（SSH）来自任意源
4. 绑定 NSG 至 frontend 子网
5. （可选）部署一台 VM 并验证网络规则效果

---

## ✅ 小测验总结（修正后）

| 题号 | 正确答案 | 说明 |
|------|-----------|------|
| 1 | A | 优先级数值小的规则优先生效 |
| 2 | C | Private Endpoint 使用专属私有 IP |
| 3 | B, C | NSG 可用于子网、NIC，不可用于 Function App 本体 |
| 4 | D | VNet 不能跨区域，子网可绑定一个 NSG（平台限制） |
| 5 | B | 精确限制特定 IP 的远程桌面访问（TCP 3389） |

---

## 📌 重点记忆

- VNet 是单区域资源
- NSG 可绑定 Subnet 或 NIC，但每个 Subnet 只能绑定一个 NSG
- Function App 的出站可受子网 NSG 控制，入站不行
- Service Endpoint ≠ Private Endpoint，后者更安全且更适用于零信任场景

---

## 📅 下一步预告 - Day 3

- 深入 Azure 虚拟机（VM）的部署选项
- 掌握高可用性架构（Availability Set / Zone / Scale Set）
- 使用 Terraform 部署多区可用 VM
