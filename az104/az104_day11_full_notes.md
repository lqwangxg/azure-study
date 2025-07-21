
# AZ-104 学习笔记 - Day 11

## 🎯 今日主题
- VNet Peering 区域内/跨区域互联机制
- VPN Gateway 部署要求与场景
- ExpressRoute 的应用特性与适用场景
- DNS 名称解析：Azure 默认、自定义与 Private DNS Zone
- 跨 VNet DNS 使用策略

---

## 🌐 VNet Peering（对等互连）

- **用途**：将两个 VNet 虚拟网络互连
- **类型**：
  - 区域内 Peering：同一区域内，零延迟
  - 跨区域 Peering：不同区域，需额外带宽费用
- **特点**：
  - 无需公网 IP
  - 默认自动互通（不需要路由表）
  - 可设置流量转发（Allow Forwarded Traffic）

---

## 🔐 VPN Gateway

- 连接场景：
  - Site-to-Site（本地与 Azure 连接）
  - Point-to-Site（远程个人设备连接 Azure）
  - VNet-to-VNet（两个 Azure VNet 加密连接）
- 必须部署在名为 `GatewaySubnet` 的专用子网中
- 会创建公用 IP，用于 VPN 通道终端

---

## 🏢 ExpressRoute

- 企业级专线接入 Azure
- 特点：
  - 避开公共互联网
  - 高可用 SLA 保证
  - 支持多区域与服务的私网通信
- 适合对延迟、数据安全要求高的场景（如金融、医疗）

---

## 📛 Azure DNS 名称解析方案

| 模式 | 描述 |
|------|------|
| 默认 Azure DNS | 自动提供 `*.internal.cloudapp.net` 名称解析，仅限 VNet 内 VM |
| 自定义 DNS | 用户可配置本地 DNS（如 AD）用于域控制与本地解析 |
| Private DNS Zone | Azure 专用 DNS 服务，可与 Private Endpoint 集成使用，支持跨 VNet 共享 |

---

## 🔐 Private DNS Zone

- 提供私有域名解析服务（如 `privatelink.blob.core.windows.net`）
- 支持自动注册 VM 名称
- 与 Private Endpoint 配合，解析到子网内部 IP
- 跨多个 VNet 使用：通过 **Virtual Network Link** 将 VNet 链接到 DNS Zone

---

## ✅ 小测验回顾（3/5）

| 题号 | 正确答案 | 解析 |
|------|-----------|------|
| 1 | C | 跨区域 Peering 支持通信，但需支付出站带宽费用 |
| 2 | B | VPN Gateway 必须部署在名为 GatewaySubnet 的子网 |
| 3 | B,C,D | Private DNS Zone 可配合 Private Endpoint 使用，支持跨 VNet |
| 4 | B | ExpressRoute 避开公共互联网，提供 SLA |
| 5 | C | 多个 VNet 应通过 VNet Link 使用同一个 Private DNS Zone |

---

## 🧠 重点记忆

- VPN Gateway ≠ 任意子网，必须是 GatewaySubnet
- ExpressRoute = 专线接入，费用高但安全可控
- 默认 DNS 仅适用于当前 VNet，Private DNS Zone 才能跨 VNet 使用
- Private Endpoint 搭配 Private DNS Zone 可实现私网 DNS 解析和访问

---

## 📌 拓展提问补充

### 1. VPN Gateway 的子网类型与区别

Azure 中 VPN Gateway 必须部署在专用子网 `GatewaySubnet` 中，且不能与其他资源共用。

| 特性 | 说明 |
|------|------|
| 子网名必须是 `GatewaySubnet` | 这是 Azure 识别并允许创建 VPN Gateway 的前提 |
| 不支持 NSG/UDR | Azure 会自动管理其网络安全与路由，不可自定义 |
| 推荐 CIDR | /27 或更大，预留足够 IP 用于高可用配置 |
| 适用于 | Site-to-Site, Point-to-Site, VNet-to-VNet, ExpressRoute Gateway 等场景 |

---

### 2. DNS Zone vs Private DNS Zone 区别

| 比较项 | DNS Zone（Public） | Private DNS Zone |
|--------|---------------------|------------------|
| 可见性 | 公网 | 私网（VNet 内部） |
| 典型用途 | 自定义域名（如 mysite.com） | VM、Storage、KeyVault 私有解析 |
| 与 Private Endpoint 集成 | 否 | 是 |
| 是否支持自动注册 | 否 | 是（VM 自动注册） |
| 跨 VNet 使用 | 否 | 是（通过 VNet Link） |

---

### 3. Azure vs AWS 网络与 DNS 服务对比

| 功能 | Azure | AWS | 差异说明 |
|------|-------|-----|-----------|
| VPN Gateway | VPN Gateway + GatewaySubnet | Virtual Private Gateway + Customer Gateway | Azure 要求专用子网，AWS 拆解更细化 |
| 专线 | ExpressRoute | Direct Connect | 功能类似，Azure 在集成方面更直接 |
| 私有 DNS | Private DNS Zone | Route53 Private Hosted Zone | 都支持私网域名解析 |
| 公共 DNS | DNS Zone | Route53 Hosted Zone | 功能相似，Azure 更适合 Azure 服务绑定 |
| 内网负载均衡 | Internal Load Balancer | NLB Internal | Azure 更适合 PaaS 集成，AWS 选择更丰富 |
| 网络互联 | VNet Peering | VPC Peering | Azure 支持转发和自动路由更方便 |

---

## 📅 下一步 - Day 12 预告

| 模块 | 内容 |
|------|------|
| 存储账户配置 | Blob、File、Queue、Table 各自特性 |
| 安全设置 | 防火墙、专用访问、SAS、加密 |
| 小测验 | 存储场景与权限配置判断 |
