# AZ-104 思维导图结构：

```pgsql
1. 身份与权限管理
   ├─ Azure AD 基础概念：Tenant / Object / Group
   ├─ RBAC 权限管理：Owner / Contributor / Reader
   ├─ Managed Identity：System / User-assigned
   └─ 多重身份认证、条件访问策略

2. 网络配置
   ├─ VNet / Subnet 构建
   ├─ NSG：规则绑定限制（子网或 NIC）
   ├─ Load Balancer / Application Gateway 区别
   ├─ Azure Firewall（DNAT/SNAT/FQDN 过滤）
   └─ Private Endpoint vs Service Endpoint

3. 计算资源管理
   ├─ VM 创建 / 备份 / Auto-shutdown / Lock
   ├─ VM Scale Set 与自动扩展设置
   ├─ Function App / Web App / AKS 概览
   └─ 计算资源监控指标：CPU、磁盘、网络

4. 存储与数据库
   ├─ 存储账户类型：Standard / Premium
   ├─ 加密机制：Platform-managed key / CMK
   ├─ Blob/SAS/Private Endpoint/Firewall 限制
   └─ Azure SQL / CosmosDB 的连接方式

5. 监控与告警
   ├─ Azure Monitor 架构图
   ├─ Metrics vs Logs 区别
   ├─ Diagnostic Settings / Activity Logs
   ├─ Log-based 告警 + Action Group
   └─ Application Insights 性能监控

6. 策略与治理
   ├─ Azure Policy vs RBAC 区别
   ├─ Initiative / 参数化策略管理
   ├─ Azure Blueprints 概述
   └─ Resource Lock、标签策略、合规性检查
```