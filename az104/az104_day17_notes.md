
# AZ-104 学习笔记 - Day 17

## 🛠 今日主题：Azure Network Watcher 与网络诊断工具

---

## 📌 Network Watcher 是什么？

Azure 提供的网络监控平台，功能涵盖：
- 端到端连接测试（Connection Troubleshoot）
- 流量审计日志（NSG Flow Logs）
- 实时包分析（Packet Capture）
- 网络拓扑图可视化（Topology）
- 路由追踪（Next Hop）
- VPN 连接状态诊断
- NSG 规则验证（IP Flow Verify）

> ⚠️ 按区域启用。建议使用 Azure Policy 确保全订阅已启用。

---

## 🔍 NSG Flow Logs（NSG 流量日志）

| 项目     | 说明 |
|----------|------|
| 日志位置 | 存储在 Azure Storage Blob 中（JSON 格式） |
| 内容     | 记录源 IP、目标 IP、端口、协议、允许/拒绝、命中的 NSG 规则 |
| 场景     | 审计网络访问、排查流量被拒原因 |
| 可视化   | 配合 Traffic Analytics 使用 |
| 等级     | Flow Logs v2 提供聚合与高级分析能力 |

---

## 🔧 常用工具功能

### ✅ IP Flow Verify
- 验证指定端口、IP 的网络流是否被 NSG 允许
- 提供被拒原因和命中的规则名称

### ✅ Connection Troubleshoot
- VM 到任意 IP/FQDN 的连通性排查
- 检测端口开放、丢包率、RTT

### ✅ Next Hop
- 查看出站流量的下一跳设备（UDR/FW/NAT 等）

### ✅ Packet Capture（抓包）
- 部署 Agent 到 VM，进行网络包的实时或定时抓取
- 用于复杂微服务故障分析

---

## 📈 网络拓扑视图

Network Watcher 支持生成资源拓扑图，包括：
- VNet、子网、NIC、NSG、VM 等组件之间的连接关系
- 可视化结构检查、规划验证

---

## 🔐 VPN 连接诊断

| 工具         | 用途 |
|--------------|------|
| VPN diagnostics | 显示隧道是否建立、IKE/ESP 状态等 |
| Azure Monitor  | 可结合日志设定告警 |
| Connection monitor | 实时追踪连接稳定性与健康状态 |

---

## 🌐 Azure、AWS、GCP 网络监视与诊断工具对比

| 维度             | Azure Network Watcher                    | AWS VPC Flow Logs & Reachability Analyzer & CloudWatch | GCP VPC Flow Logs & Network Intelligence Center      |
|------------------|------------------------------------------|-------------------------------------------------------|-----------------------------------------------------|
| 流量日志         | NSG Flow Logs（存储于 Blob，可分析）     | VPC Flow Logs（存储于 CloudWatch/ S3）                | VPC Flow Logs（存储于 Logging，可分析）             |
| 连通性测试       | Connection Troubleshoot、IP Flow Verify  | Reachability Analyzer                                 | Connectivity Tests                                  |
| 路由分析         | Next Hop、Effective Routes               | Route Table 分析工具                                  | 路由表可视化、路由分析                              |
| 抓包功能         | Packet Capture（需安装 Agent）           | 无原生抓包，需自建工具                                | Packet Mirroring（流量镜像，需自建分析）            |
| 拓扑可视化       | 网络拓扑图（Topology）                   | VPC Dashboard、第三方工具                             | Network Topology（Network Intelligence Center）      |
| VPN 诊断         | VPN Connection Diagnostics               | VPN CloudWatch Metrics、状态监控                      | VPN Monitoring（Metrics、日志）                     |
| 集成告警         | 可与 Azure Monitor/Log Analytics 集成     | 可与 CloudWatch/CloudTrail 集成                        | 可与 Cloud Monitoring 集成                          |
| 自动化与API      | 支持 ARM、CLI、REST API                  | 支持 CLI、SDK、API                                    | 支持 gcloud、API、Terraform                         |

**总结：**
- 三大云厂商均提供流量日志、连通性测试、路由分析、拓扑可视化等网络监控与诊断能力。
- Azure Network Watcher 功能集中、界面友好，抓包和拓扑可视化较突出。
- AWS 工具分散，需结合 VPC Flow Logs、Reachability Analyzer、CloudWatch 等使用。
- GCP Network Intelligence Center 提供统一入口，支持拓扑、连通性、流量日志等多维度分析。
- 建议结合实际云平台和运维需求选择合适的网络监控方案
---

## ✅ 小测验回顾（4 / 5）

### 1. Network Watcher 提供的功能包括？✅

- ✅ B. 网络连通性测试  
- ✅ C. 流量日志收集  
- ✅ D. IP 流量验证  
- ❌ A. VM 性能监控 → 属于 Azure Monitor

---

### 2. 查看 NSG 是否允许特定流量？✅

- ✅ B. IP Flow Verify

---

### 3. NSG Flow Logs 内容包括？⚠️

- ✅ B. 访问源与目标 IP  
- ✅ C. 使用的端口与协议  
- ✅ D. 匹配的 NSG 规则  
- ❌ A. 不记录网络包内容（不是 PCAP）

---

### 4. 检查出站流量实际出口路径？✅

- ✅ B. Next Hop

---

### 5. 关于 Network Watcher 正确说法？⚠️

- ✅ A. 按区域启用  
- ✅ B. 可抓取网络数据包（Packet Capture）  
- ✅ D. 支持网络拓扑图  
- ❌ C. 不用于配置 NSG，只做分析

---

## 🧠 今日重点记忆

- Network Watcher 是 Azure 核心的网络诊断平台
- NSG Flow Logs 用于审计和故障排查
- IP Flow Verify / Next Hop 是日常必备排障工具
- VPN Connection Diagnostics 可快速检查隧道连接状态
- 拓扑图与包捕获适用于结构审查与精细排障

---

## 📅 下一步预告 - Day 18

| 模块                         | 内容                             |
|------------------------------|----------------------------------|
| Azure Storage 架构与冗余选项 | LRS / ZRS / GRS / RA-GRS        |
| 生命周期管理与性能优化       | 热/冷/存档访问层                |
| 安全访问方式                 | SAS / RBAC / 网络限制 / 加密     |
