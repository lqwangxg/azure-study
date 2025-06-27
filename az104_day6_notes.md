
# AZ-104 学习笔记 - Day 6

## 🎯 学习目标
- 理解网络安全组（NSG）与应用安全组（ASG）的作用与配置
- 掌握 Azure Firewall 的功能与适用场景
- 区分 Service Endpoint / Private Endpoint / Private Link
- 理解负载均衡器与防火墙的功能差异及费用考量
- 完成网络安全相关测验

---

## 🔐 网络安全组（NSG）

| 属性 | 说明 |
|------|------|
| 控制层级 | 可应用于子网 / NIC |
| 规则组成 | 优先级、方向、协议、端口、源/目标、操作 |
| 默认规则 | 允许 VNet 内部通信，拒绝所有入站 Internet |

> ✅ 优先级数值越小，优先级越高（范围：100-4096）

---

## 🧰 应用安全组（ASG）

| 特点 | 用途 |
|------|------|
| 逻辑分组 VM | 用组名代替 IP 写入 NSG 规则 |
| 精细化管理 | 简化大型部署中的网络安全策略 |

---

## 🔥 Azure Firewall

| 特性 | 描述 |
|------|------|
| 状态检测防火墙 | 支持 TCP/UDP + FQDN/URL 级过滤 |
| 应用规则 | 可配置 HTTP/S 应用规则（如只允许访问 *.microsoft.com） |
| NAT 支持 | 支持 SNAT（出站）与 DNAT（入站） |
| 集中控制 | 可统一管理多个 VNet 流量 |

---

## 🌐 私有访问机制比较

| 类型 | 描述 | 优点 | 局限 |
|------|------|------|------|
| **Service Endpoint** | Azure 资源虚拟网络访问通道 | 配置简单 | 暴露服务公网地址 |
| **Private Endpoint** | 分配给服务的专属私网 IP | 更安全（关闭公网） | 需要私有 DNS 支持 |
| **Private Link** | Private Endpoint 的底层支持 | 支持第三方资源 | DNS 解析需额外配置 |

---

## ⚖️ Azure Firewall vs Load Balancer

| 比较项 | Azure Firewall | Azure Load Balancer |
|--------|----------------|---------------------|
| 主要用途 | 安全策略 + NAT（SNAT/DNAT） | 分发流量，提升可用性 |
| 层级 | L3-L7（状态检测） | L4（TCP/UDP） |
| 访问控制 | 支持 URL/FQDN 白名单、端口过滤 | 无访问控制 |
| SNAT 支持 | ✅（出站伪装） | ✅（出站 NAT 规则） |
| DNAT 支持 | ✅（入站映射） | ✅（入站规则） |
| 多区域集中控制 | ✅（支持多个 VNet） | ❌（区域隔离） |
| 成本 | 高（有固定费用 + 流量计费） | 低（Standard SKU 按规则和数据流量计费） |
| 日志审计 | ✅（集成 Log Analytics） | ❌（无内建日志） |

✅ 建议：负载分发选 Load Balancer，安全控制选 Firewall，两者可结合使用。

---

## 🛠️ 实操命令回顾

```bash
# 创建 NSG 并添加 SSH 规则
az network nsg create -g myRG -n myNSG

az network nsg rule create -g myRG --nsg-name myNSG   -n allow-ssh --priority 1000 --direction Inbound   --access Allow --protocol Tcp --destination-port-ranges 22   --source-address-prefixes Internet --destination-address-prefixes '*'

# 创建 Private Endpoint 示例
az network private-endpoint create   --name pe-blob --resource-group myRG   --vnet-name myVNet --subnet mySubnet   --private-connection-resource-id /subscriptions/xxx/resourceGroups/xxx/providers/Microsoft.Storage/storageAccounts/myBlob   --group-id blob --connection-name pe-blob-conn
```

---

## 🧪 Day 6 测验回顾（5/5 满分 🎉）

| 题号 | 正确答案 | 说明 |
|------|-----------|------|
| 1 | C | NSG 默认允许 VNet 内通信，拒绝 Internet 入站 |
| 2 | C | 数字越小优先级越高 |
| 3 | A | Private Endpoint 为资源分配私网 IP |
| 4 | B | ASG 用于逻辑分组替代 IP 写入 NSG |
| 5 | A, C, D | Firewall 支持 FQDN、集中控制、NAT 规则 |

---

## 📌 总结记忆重点

- **NSG + ASG** 控制 VNet 流量是基础安全手段
- **Firewall** 提供 L3-L7 的深度控制，适合多网络统一防护
- **Private Endpoint** 是高安全场景（如 Key Vault / Storage）的首选私网访问方式
- **负载均衡 vs 防火墙**：前者为分流、后者为控制，二者可组合部署

---

## 📅 下一步 - Day 7 预告

| 模块 | 内容 |
|------|------|
| 监控平台 | Log Analytics、Application Insights |
| 告警机制 | Metric Alerts、Activity Log Alerts |
| 自动化 | AutoScale、ARM Template、Terraform |
| 小测验 | 操作日志分析、自动缩放策略题 |
