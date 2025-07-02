# AZ-104 学习笔记 - Day 10

## 🎯 学习目标
- 掌握虚拟网络（VNet）架构与子网划分
- 理解网络安全组（NSG）规则配置与优先级
- 学习用户定义路由（UDR）控制流量路径
- 区分服务终结点（Service Endpoint）与私有链接（Private Link）
- 通过测验理解实际配置与限制

---

## 🌐 虚拟网络（VNet）

| 项目 | 内容 |
|------|------|
| 地址空间 | 使用 CIDR，如 10.0.0.0/16 |
| 子网划分 | 不可重叠；用于逻辑隔离资源 |
| 应用范围 | VM / App / Function 等资源 |
| 对接能力 | VPN / ExpressRoute / Peering 支持跨地域互联 |

---

## 🧱 子网划分（Subnet）

- 每个子网需预留地址（Azure 使用 5 个 IP）
- 示例：/23 提供约 512 个 IP（适合 500 台 VM）
- 子网级别可绑定 NSG、UDR、Private Endpoint

---

## 🔒 网络安全组（NSG）

- 用于控制 Inbound / Outbound 流量
- 每条规则含：方向、源/目标、端口、协议、优先级、是否允许

### ⚙️ NSG 规则优先级
- 数值越小 → 优先级越高（100 > 200）
- 默认规则（优先级 65000）允许虚拟网络内部流通，拒绝 Internet 入站

---

## 🧭 用户定义路由（UDR）

| 类型 | 说明 |
|------|------|
| 系统路由 | 自动添加本地、Internet、VNet 路由 |
| UDR | 自定义转发路径（如通过防火墙） |
| Blackhole | 发往目标的流量被丢弃（用于隔离） |

---

## ☁️ 服务终结点（Service Endpoint）

| 特征 | 描述 |
|------|------|
| 接入方式 | 启用子网 → 支持服务识别来源 |
| 识别机制 | 服务识别来自特定 VNet 请求 |
| 适用服务 | Storage、SQL、Key Vault 等 |
| 注意事项 | 不会关闭公网访问，仅作为来源验证手段 |

---

## 🔐 私有链接（Private Link / Endpoint）

| 特征 | 描述 |
|------|------|
| 接入方式 | 在子网中创建 Private Endpoint |
| 通信方式 | 使用子网内部 IP 通信 |
| 效果 | 屏蔽服务公网访问，仅限私网访问 |
| 场景 | 高安全性访问 Storage、Key Vault、Cosmos DB 等服务 |

---

## 🔍 服务终结点 vs Private Endpoint 对比

| 比较项 | 服务终结点 | 私有链接 |
|--------|-------------|-----------|
| 使用私网 IP | ❌ 否 | ✅ 是 |
| 阻止公网访问 | ❌ 否 | ✅ 是 |
| 设置复杂度 | 简单 | 相对复杂 |
| 适用性 | 来源标识与防火墙结合使用 | 安全隔离、审计要求高的环境 |

---

## ✅ 小测验回顾（3.5/5）

| 题号 | 正确答案 | 解析 |
|------|-----------|------|
| 1 | C | /23 提供 ~512 IP，适合 500 台 VM |
| 2 | B | 优先级数值越小越高 |
| 3 | B | 拒绝 443 入站 → 无法访问 HTTPS 服务 |
| 4 | A,C,D | 服务终结点不会自动关闭公网访问 |
| 5 | A,C,D | Private Endpoint 映射私网 IP，可强制私网访问 |

---

## 🧠 记忆重点

- 子网划分需根据容量合理设定 CIDR，如 /23、/24 等
- NSG 控制出入规则，注意优先级与默认规则
- UDR 可定向流量经特定设备或“黑洞”
- 服务终结点只做来源验证，不能关闭公网
- 私有链接可为服务创建专属 IP，实现完全私网访问

---

## 🌐 Azure 与 AWS 网络资源对比

| 维度         | Azure                                  | AWS                                      |
|--------------|----------------------------------------|------------------------------------------|
| 虚拟网络     | VNet，支持子网、对等互联、VPN、ER      | VPC，支持子网、Peering、VPN、Direct Connect |
| 子网         | 必须在 VNet 下创建，需预留 5 个 IP      | 必须在 VPC 下创建，需预留 5 个 IP         |
| 网络安全组   | NSG，支持子网/网卡级别，默认拒绝入站   | Security Group，实例/接口级别，默认拒绝入站 |
| 路由         | UDR（用户定义路由），支持黑洞路由       | Route Table，支持自定义路由和黑洞         |
| 服务终结点   | Service Endpoint，来源验证，不关公网    | VPC Endpoint（Gateway/Interface），可关闭公网 |
| 私有链接     | Private Link/Endpoint，专属私网 IP      | VPC Endpoint（Interface），专属私网 IP    |
| DNS          | Azure DNS、Private DNS Zone            | Route 53、Private Hosted Zone            |
| 跨网互联     | VNet Peering，支持全局                  | VPC Peering，区域内/跨区域                |
| 高级功能     | 支持防火墙、DDoS、网络虚拟设备          | 支持防火墙、DDoS、网络虚拟设备            |

**总结：**
- Azure VNet ≈ AWS VPC，核心概念类似，均支持子网、路由、网络安全组等功能。
- Azure NSG 与 AWS Security Group 都可细粒度控制流量，但 Azure 支持子网和网卡双层绑定。
- 服务终结点和私有链接在两大云厂商均有实现，AWS VPC Endpoint 可直接关闭公网访问，Azure Service Endpoint 仅做来源验证，Private Link 可实现完全私网。
- 路由、DNS、互联等高级特性两者均支持，具体实现和命名略有差异，建议结合实际需求

---

## 📅 下一步 - Day 11 预告

| 模块 | 内容 |
|------|------|
| 网络互联 | VNet Peering / VPN Gateway / ExpressRoute |
| DNS 与名称解析 | 内部 DNS、自定义 DNS、Private DNS Zone |
| 应用场景分析 | 多区域访问、跨订阅通信 |
| 小测验 | 网络互联与名称解析配置判断 |

