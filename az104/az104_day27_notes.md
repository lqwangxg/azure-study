
# AZ-104 学习笔记 - Day 27

## 🔐 网络安全控制：NSG、ASG 与网络流日志

---

## 🚧 网络安全组（NSG）

- 用于控制子网或 NIC 的入站与出站网络访问。
- 每条规则包括方向、源/目标地址、端口、协议、动作（允许/拒绝）等。
- **默认规则**存在于所有 NSG：
  - AllowVNetInbound
  - AllowAzureLoadBalancerInbound
  - DenyAllInbound
- **优先级范围：100~4096，数值越小越优先执行**

### 默认行为：
- 默认拒绝所有入站流量（除非显式允许）
- 默认允许所有出站流量（除非显式拒绝）
- NIC 与子网上的 NSG 会**叠加评估**

---

## 👥 应用安全组（ASG）

- ASG 用于逻辑分组 VM（通过网卡 NIC 添加）
- NSG 可将 ASG 作为源/目标地址，提升规则的可维护性
- 避免硬编码 IP 地址，便于自动扩缩容场景

---

## 📊 NSG Flow Logs 与分析工具

- **NSG Flow Logs** 可记录每一次 IP 层通信，包含：
  - 源地址、目标地址、端口、协议、方向、是否允许等
- 前提条件：
  - 存储账户
  - 启用诊断设置
  - 已创建 NSG

### 分析方式：

| 工具            | 功能 |
|------------------|------|
| Network Watcher  | 启用日志、查看流数据 |
| Traffic Analytics | 可视化流量来源、方向、端口 |
| KQL + Log Analytics | 自定义分析和告警触发 |
---

## 🌐 Azure、AWS、GCP 网络安全控制（NSG、ASG 与网络流日志）对比

| 维度           | Azure                                      | AWS                                         | GCP                                         |
|----------------|--------------------------------------------|---------------------------------------------|---------------------------------------------|
| 网络安全组     | NSG（Network Security Group）              | Security Group                              | Firewall Rules                              |
| 作用范围       | 子网、NIC                                  | 实例、ENI                                   | VPC、子网、实例                             |
| 规则类型       | 入站/出站，优先级明确（100~4096）           | 入站/出站，按规则顺序评估                   | 入站/出站，优先级明确（0~65535）            |
| 默认行为       | 默认拒绝入站，允许出站                     | 默认拒绝入站，允许出站                      | 默认拒绝入站，允许出站                      |
| 应用安全组     | ASG（Application Security Group，逻辑分组） | 无原生 ASG，常用标签+Security Group组合      | 无原生 ASG，常用标签+防火墙规则组合         |
| 动态分组       | 支持（ASG 绑定 VM 网卡，规则用 ASG 代替 IP）| 需手动维护标签和 Security Group             | 需手动维护标签和防火墙规则                  |
| 网络流日志     | NSG Flow Logs（需存储账户，支持分析工具）   | VPC Flow Logs（CloudWatch/S3，支持分析）     | VPC Flow Logs（Cloud Logging，支持分析）     |
| 日志分析工具   | Network Watcher、Traffic Analytics、KQL     | CloudWatch Logs Insights、Athena、第三方工具 | Cloud Logging、BigQuery、Network Intelligence Center |
| 细粒度控制     | 支持基于 IP、端口、协议、ASG                | 支持基于 IP、端口、协议、标签                | 支持基于 IP、端口、协议、标签                |

**总结：**
- 三大云厂商均支持网络安全组（NSG/Security Group/Firewall Rules）和网络流日志，默认行为类似。
- Azure 独有 ASG 动态分组，简化大规模 VM 管理，AWS/GCP 需结合标签实现类似效果。
- 流日志均可与各自日志分析平台集成，便于安全审计与流
---

## ✅ 小测验回顾（4 / 5）

### 1. 关于 NSG 默认行为？

- ✅ C. 子网与 NIC 上的 NSG 会叠加评估

### 2. NSG 规则优先级？

- ❌ C（错误）  
- ✅ 正解：A. 默认规则优先级高于自定义规则，数字小优先，不能相同

### 3. ASG 的优点？

- ✅ B. 替代 IP 地址进行分组  
- ✅ C. 简化 NSG 配置

### 4. 启用 Flow Logs 的条件？

- ✅ A. 已存在 NSG  
- ✅ B. 启用诊断设置  
- ✅ D. 存储账户支持写入日志

### 5. 分析 Flow Logs 的工具？

- ✅ B. Network Watcher  
- ✅ C. Traffic Analytics

---

## 📅 下一步预告 - Day 28

| 模块 | 内容 |
|------|------|
| Azure 网络通信机制 | Service Endpoint vs Private Link |
| Azure Load Balancer 类型与场景 | Basic / Standard / Internal |
| DNS 与名称解析控制 | Azure DNS、Private DNS Zone |
