# ✅ Day 12 知识点总结：Azure Bastion、RBAC、JIT、访问控制

## Azure Bastion
- 安全访问 Azure VM，无需暴露 RDP/SSH 端口。
- 使用浏览器远程连接，默认加密流量。
- 无需配置公网 IP 地址。

## Just-In-Time VM Access
- 与 Defender for Cloud 配合使用。
- 通过请求机制临时开放 RDP/SSH。
- 可限制访问时间与来源 IP，提升安全性。

## Azure RBAC 权限控制
- 可设置在管理组、订阅、资源组、资源级别。
- 支持继承权限模型。
- 常见内建角色：Owner、Contributor、Reader。

### 自定义角色（Custom Role）
- 使用 JSON 定义。
- 可精确指定操作（Actions）、资源（Scopes）。
- 用于无法满足内建角色权限粒度的场景。

## Resource Locks（资源锁）
- CanNotDelete：资源不能被删除。
- ReadOnly：资源只能读取，不能修改。
- 常用于防止误删关键资源。

## Key Vault 权限控制
- Key Vault Secrets User：仅查看密钥和秘密。
- Key Vault Contributor：可管理内容但不能访问实际机密。
- Key Vault Administrator：拥有全部管理权限。

## TLS over WebSocket
- Azure Bastion 使用的传输协议。
- 保证跨浏览器兼容和安全性。

---

## ❌ Day 12 错题集（AZ-500）

### 第 3 题  
**问题**：下列哪项不是 Azure RBAC 的作用范围？  
**你的答案**：C  
**正确答案**：C（NSG 规则不是 RBAC 管理的）  
**解析**：Azure RBAC 仅适用于管理组、订阅、资源组及资源层，NSG 属于网络级配置，不属于 RBAC 控制范围。
