
# AZ-104 学习笔记 - Day 16

## 🌐 今日主题：Azure VPN Gateway 与混合网络架构

---

## 📌 Azure VPN Gateway 概览

- Azure 提供的托管型 VPN 设备，用于连接本地与云端、多个 Azure 网络之间
- 功能包括：
  - Site-to-Site VPN（站点对站点）
  - Point-to-Site VPN（远程客户端接入）
  - VNet-to-VNet VPN（虚拟网络互连）
- 部署要求：
  - 必须部署在名为 `GatewaySubnet` 的专用子网
  - 每个 VNet 最多一个 VPN Gateway
  - 子网中不可有其他资源

---

## 🔗 Site-to-Site VPN

| 项目 | 内容 |
|------|------|
| 场景 | 本地网络与 Azure VNet 互联 |
| 需求 | 本地公网 IP、本地 VPN 设备 |
| 协议 | IPSec / IKEv1 / IKEv2 |
| 配置 | 配置 Local Network Gateway 作为对端定义 |
| 特点 | 长期稳定连接，适合企业混合云 |

---

## 👤 Point-to-Site VPN

| 项目 | 内容 |
|------|------|
| 场景 | 员工远程访问 Azure 网络 |
| 协议 | IKEv2、SSTP、OpenVPN |
| 认证 | 根证书认证或 Azure AD 身份认证 |
| 特点 | 简单易用，灵活性高，支持多平台 |

---

## 🌉 VNet-to-VNet VPN

- 场景：将两个不同的 Azure 虚拟网络通过加密隧道互连
- 适用：跨区域、跨订阅的网络互通
- 原理：类似 Site-to-Site，需要双向定义 Local Network Gateway

---

## 💡 VPN Gateway SKU 比较

| SKU      | 吞吐量（理论） | 特点 |
|----------|----------------|------|
| Basic    | ~100 Mbps      | 入门型，不支持高可用 |
| VpnGw1   | ~650 Mbps      | 常规使用推荐 |
| VpnGw2~5 | 高达 10 Gbps   | 适用于高流量、跨国业务 |

> ⚠️ 实际带宽会受到加密负载、网络稳定性、并发连接数影响。

---

## 🛣 ExpressRoute vs VPN Gateway

| 项目         | ExpressRoute            | VPN Gateway                |
|--------------|-------------------------|----------------------------|
| 链接方式     | 专线（物理连接）        | 公网 IPSec 隧道            |
| 带宽         | 50 Mbps ~ 10 Gbps       | Up to 10 Gbps             |
| SLA          | 高，可达 99.95%+        | 不保证（依赖公网）         |
| 成本         | 高（含服务+线路+MSEE）  | 中等（按小时 + 传输量）     |
| 安全性       | 物理隔离                | 加密保护                   |
| 应用场景     | 金融、政府、大型企业    | 中小企业、移动办公         |

---

## 🌐 Azure、AWS、GCP 关于 VPN 与混合网络的对比

| 维度             | Azure                                  | AWS                                      | GCP                                      |
|------------------|----------------------------------------|------------------------------------------|------------------------------------------|
| 托管 VPN 网关    | VPN Gateway                            | VPN Gateway                              | Cloud VPN                                |
| 站点对站点 VPN   | Site-to-Site VPN                       | Site-to-Site VPN                         | HA VPN / Classic VPN                     |
| 客户端 VPN       | Point-to-Site VPN                      | Client VPN                               | 支持 IKEv2/L2TP，推荐第三方或自建         |
| 虚拟网络互联     | VNet-to-VNet VPN                       | VPC Peering / VPN                        | VPC Network Peering / VPN                |
| 专线服务         | ExpressRoute                            | Direct Connect                           | Dedicated Interconnect / Partner Interconnect |
| 部署要求         | 专用 GatewaySubnet，命名要求            | 专用子网，无强制命名                     | 专用子网，无强制命名                      |
| 支持协议         | IKEv1/IKEv2/IPSec/SSTP/OpenVPN         | IKEv1/IKEv2/IPSec/OpenVPN                | IKEv1/IKEv2/IPSec                        |
| 认证方式         | 证书、Azure AD、RADIUS                 | 证书、Active Directory、RADIUS           | 证书、预共享密钥、Cloud IAM              |
| 高可用性         | 支持主动-被动/主动-主动，部分 SKU 支持   | 支持多可用区冗余                         | HA VPN 支持高可用                        |
| 最大带宽         | Up to 10 Gbps（高 SKU）                | Up to 10 Gbps（高规格）                  | Up to 3 Gbps（HA VPN）                   |
| 计费模式         | 按网关规格+流量计费                    | 按网关+流量计费                          | 按隧道+流量计费                          |
| 管理与监控       | Network Watcher，诊断工具               | VPC Flow Logs，CloudWatch                | VPC Flow Logs，Cloud Monitoring          |

**总结：**
- 三大云厂商均提供托管 VPN 网关，支持站点对站点、客户端 VPN 及专线混合云方案。
- Azure Point-to-Site VPN 支持多协议和 Azure AD 认证，AWS Client VPN 支持 OpenVPN，GCP 客户端 VPN 需自建或用第三方。
- 专线服务（ExpressRoute/Direct Connect/Interconnect）均适合高带宽、低延迟场景。
- 高可用、带宽、协议支持等细节略有差异，建议结合实际业务需求和平台特性
---

## ✅ 小测验回顾（3 / 5）

### 1. 远程办公建议方式？✅

- ✅ C. Point-to-Site VPN

---

### 2. VPN Gateway 的部署要求？✅

- ✅ A. 需部署在 GatewaySubnet 子网中
- ✅ C. 子网名称必须为 GatewaySubnet

---

### 3. Site-to-Site VPN 的特性？✅

- ✅ A. 加密隧道
- ✅ B. 本地公网 IP
- ✅ D. 配置 Local Network Gateway

---

### 4. 连接两个跨区域 VNet？❌

- ❌ A. VNet Peering 不支持跨 region
- ✅ B. 应使用 VNet-to-VNet VPN

---

### 5. Point-to-Site 支持协议？❌

- ✅ A. IKEv2
- ✅ B. OpenVPN
- ✅ D. SSTP
- ❌ C. IPSec/IKE 是 Site-to-Site 使用的协议

---

## 🧠 今日重点记忆

- VPN Gateway 是跨网络加密连接的核心组件
- Site-to-Site：本地对接 VPN，适合固定办公环境
- Point-to-Site：远程访问首选，适合 BYOD 员工
- ExpressRoute 是高 SLA、高带宽专线，成本高
- 不同类型连接可组合形成混合云架构

---

## 📅 下一步预告 - Day 17

| 模块 | 内容 |
|------|------|
| Network Watcher |
| NSG 流量日志分析 |
| VPN 诊断与连通性排查 |
