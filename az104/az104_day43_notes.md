
# AZ-104 学习笔记 - Day 43

## 🌐 网络诊断与 KQL 查询实战

---

## 📄 NSG Flow Logs（网络流量日志）

### ✅ 功能
- 记录 NSG 所允许或拒绝的入站/出站连接
- 包含 5-tuple 信息：
  - 源 IP / 端口
  - 目标 IP / 端口
  - 协议（TCP/UDP）
  - 操作（Allow / Deny）

### ✅ 启用方式
- 需手动启用
- 输出目标为：
  - Azure Storage（Blob）
  - Log Analytics 工作区（用于查询分析）

---

## 🛠 Network Watcher 工具集

| 工具名称               | 功能说明                                 |
|------------------------|------------------------------------------|
| Connection Troubleshoot | 测试端到端连接是否被防火墙/NSG/UDR 阻断 |
| Packet Capture         | 在 VM 上抓取特定端口或协议的网络包       |
| IP Flow Verify         | 检查流量是否被当前网络规则阻断或放行     |
| Next Hop               | 查看流量发往目标 IP 时的下一跳设备       |

> 🔍 用于定位网络连通性问题、确认安全规则是否生效

---

## 📡 Network Performance Monitor（NPM）

### ✅ 功能
- 检测 Azure 网络与本地网络间的：
  - 延迟
  - 丢包
  - 可用性
- 显示网络拓扑图、瓶颈路径

### ✅ 要求
- 部署 Log Analytics Agent
- 启用 NPM 解决方案包

---

## 🔎 KQL 查询实战

### ✅ summarize 聚合函数
- 类似 SQL 的 GROUP BY + AVG / COUNT 等聚合
```kql
Perf
| where ObjectName == "Processor"
| summarize avg(CounterValue) by bin(TimeGenerated, 5m)
```

### ✅ join 表连接
- 用于将多个日志表组合分析
```kql
Heartbeat
| join kind=inner (
  VMConnection
) on Computer
```

### ✅ make-series 用法
- 用于构建时间序列折线图
- 自动填补缺失时间段为 0
```kql
Perf
| where ObjectName == "Processor"
| make-series avg(CounterValue) default=0 on TimeGenerated from ago(1h) to now() step 5m by Computer
```

---

## ✅ 小测验复盘（得分：14 / 20）

### 问题 1：NSG Flow Log
✅ A, C, D  
❌ B：不存储于 Azure SQL，而是 Storage 或 Log Analytics

### 问题 2：Network Watcher 工具
✅ A, B, D  
❌ C：不调试 AKS 网络策略

### 问题 3：NPM 网络性能监控
✅ A, B, D  
❌ C：不是内建服务，需手动部署

### 问题 4：KQL - summarize/join
✅ A, B  
❌ C：summarize ≠ WHERE  
❌ D：join 默认是 inner

### 问题 5：make-series 函数
✅ A, C, D  
❌ B：NSG 配置状态不适用 make-series

---

## 🔜 下一步预告 - Day 44 模块

| 模块                     | 内容说明                             |
|--------------------------|--------------------------------------|
| Azure Key Vault 集成     | 密钥/证书管理、系统分配身份验证       |
| 托管标识（MSI）原理      | System-assigned 与 User-assigned 区别 |
| RBAC + KeyVault + Function 实战 | 权限继承、最小权限访问               |
