
# AZ-104 学习笔记 - Day 15

## 🌐 今日主题：Azure DNS 与名称解析架构

---

## 📌 Azure DNS（公网 DNS）

- 用于注册和管理公网可访问的域名（如 `contoso.com`）
- 支持记录类型：A、CNAME、TXT、MX、SRV
- **仅提供权威解析服务，不支持递归解析**
- 无法用于 VNet 内部名称解析或 VM 自动注册

---

## 📌 Private DNS Zone（私有 DNS 区域）

用于在 Azure VNet 内部提供 DNS 服务：

| 特性         | 描述 |
|--------------|------|
| 区域示例     | `priv.contoso.com` |
| 自动注册     | VM 开启 DNS 自动注册后，可自动创建 A 记录 |
| 多 VNet 支持 | 一个 Private DNS Zone 可绑定多个 VNet |
| 内部解析     | 所有绑定的 VNet 内 VM 可互相解析名称 |
| 安全性       | 支持 Azure RBAC 控制访问权限 |

> ⚠️ 不支持公网解析，也不能作为递归 DNS Server

---

## 📌 Azure 默认 DNS 解析服务

- 默认虚拟 IP：**168.63.129.16**
- 所有 VNet 共享此地址作为默认 DNS
- 自动解析 `*.internal.cloudapp.net`
- 支持：
  - 自动注册 VM 名称（如果使用 Private DNS）
  - 与 Log Analytics 等服务集成

---

## 📌 自定义 DNS Server（本地部署）

自建 DNS 服务器，如 Windows Server DNS，适用于复杂控制需求：

| 特性           | 描述 |
|----------------|------|
| 成本与维护     | 需部署 VM，手动配置 |
| 动态注册支持   | 可通过 DHCP 实现 |
| 灵活性         | 支持自定义转发、递归配置 |
| 与 Azure 配合  | 需通过 UDR 控制查询路径 |
| 注意事项       | 不能使用 168.63.129.16 作为转发目标 |

---

## 🌐 Azure、AWS、GCP DNS 服务对比

| 维度           | Azure DNS                        | AWS Route 53                        | GCP Cloud DNS                      |
|----------------|----------------------------------|-------------------------------------|------------------------------------|
| 服务类型       | 托管 DNS 区域（公有/私有）       | 托管 DNS 区域（公有/私有）          | 托管 DNS 区域（公有/私有）         |
| 私有 DNS 支持  | 支持 Private DNS Zone            | 支持 Private Hosted Zone            | 支持 Private DNS Zone              |
| 集成能力       | 与 VNet 集成，支持自定义解析      | 与 VPC 集成，支持自定义解析         | 与 VPC 网络集成，支持自定义解析    |
| 记录类型       | A、AAAA、CNAME、MX、TXT、SRV 等  | A、AAAA、CNAME、MX、TXT、SRV 等     | A、AAAA、CNAME、MX、TXT、SRV 等    |
| 流量管理       | 支持流量路由（Traffic Manager）   | 支持多种流量策略（加权、延迟等）    | 支持地理、延迟等流量策略           |
| 健康检查       | 需结合 Traffic Manager            | 内置健康检查                        | 需结合负载均衡或自定义             |
| DNSSEC         | 支持                             | 支持                                | 支持                               |
| API/自动化     | 支持 ARM、CLI、PowerShell、REST   | 支持 AWS CLI、API、CloudFormation   | 支持 gcloud、API、Terraform        |
| 计费           | 按区域数和查询量计费              | 按区域数和查询量计费                | 按区域数和查询量计费               |

**总结：**
- 三大云厂商均提供公有和私有 DNS 托管服务，支持丰富的记录类型和自动化接口。
- Azure DNS 与 VNet 集成，AWS/GCP 与 VPC 集成，均可实现云内私有解析。
- AWS Route 53 在流量管理和健康检查方面功能更丰富，Azure 可结合 Traffic Manager 实现类似能力。
- DNSSEC、API 自动化、计费模式基本一致，
---

## ✅ 常见对比总结

| 功能                     | Azure DNS | Private DNS Zone | 自定义 DNS Server |
|--------------------------|-----------|------------------|-------------------|
| 公网域名解析             | ✔️        | ❌               | ❌                |
| 内网 VM 自动注册         | ❌        | ✔️               | 部分支持          |
| 多 VNet 支持             | ❌        | ✔️               | 需手动配置转发     |
| RBAC 权限控制            | ✔️        | ✔️               | ❌                |
| 运维成本                 | 最低      | 很低             | 高（需自管）       |

---

## ✅ 小测验回顾（4.5 / 5）

### 1. Azure DNS 的功能？（A, C 正确）
- ✅ A：公网 DNS 服务
- ✅ C：支持 A / CNAME / MX 管理
- ❌ B：不支持自动注册 VM
- ❌ D：不能用于 VNet 内部解析

---

### 2. Private DNS Zone 的优势？（A, C, D 正确）
- ✅ A：跨 VNet 自动解析
- ✅ C：配置简单，无需 DNS VM
- ✅ D：VM 支持自动注册 A 记录
- ❌ B：不支持递归解析

---

### 3. VNet 默认 DNS？
- ✅ C：168.63.129.16

---

### 4. 跨 VNet 互相解析名称推荐方法？
- ✅ B：Private DNS Zone + 多 VNet 绑定

---

### 5. 关于自定义 DNS 的正确说法？
- ✅ A：需要自建 DNS VM
- ✅ B：可用于内网服务解析
- ✅ D：可结合 UDR 控制查询流量路径
- ❌ C：与 Bastion 无关

---

## 🧠 今日重点记忆

- Azure DNS 是公网解析服务，不能用于内网
- Private DNS Zone 是替代自建 DNS 的最佳方式
- 默认 DNS IP 为 168.63.129.16（平台提供递归服务）
- 跨 VNet 的 DNS 解析建议使用 Private DNS Zone
- 自定义 DNS 可结合 UDR 与本地 AD 实现混合解析

---

## 📅 下一步预告 - Day 16

| 模块 | 内容 |
|------|------|
| VPN Gateway 原理与配置 |
| Site-to-Site VPN、Point-to-Site VPN 区别 |
| ExpressRoute 与混合网络设计 |
