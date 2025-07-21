
# AZ-104 学习笔记 - Day 44

## 🔐 Azure Key Vault 与托管身份（Managed Identity）

---

## 🏦 Azure Key Vault 功能总结

### ✅ 支持的用途
| 功能类别   | 说明                                                         |
|------------|--------------------------------------------------------------|
| 密钥管理   | 存储对称/非对称密钥、与 Azure 加密服务集成                  |
| 机密管理   | 存储字符串类型的敏感信息，如数据库连接串、API 密钥等        |
| 证书管理   | TLS/SSL 证书存储与轮换，支持绑定 CA                          |
| 审计日志   | 所有操作可记录日志，输出到 Log Analytics、Event Hub 等       |

> 🔒 支持 RBAC 模式和访问策略模式（二选一）

---

## 👤 托管身份（Managed Identity）

### ✅ 类型区分
| 类型                | 特点说明                                                   |
|---------------------|------------------------------------------------------------|
| System-assigned     | 与资源一一绑定，删除资源后身份自动删除                     |
| User-assigned       | 独立资源，可被多个资源共用                                 |

### ❗ 常见误解
- ❌ 托管身份不具备密码或 Secret，不可用于登录
- ✅ 仅可用于 **资源间**安全调用，不涉及用户权限

---

## 🔐 与 Key Vault 的授权方式

### 方式一：RBAC 模式（推荐）
- 分配内建角色（如 Key Vault Secrets User）
- 适合大规模权限统一管理
- 支持 Azure AD 的最小权限原则

### 方式二：访问策略模式
- 手动添加身份（User/System-assigned MI）
- 明确列出 Secret / Key / Certificate 的操作权限

> 💡 两种模式不能同时启用，需创建 Vault 时设定

---

## ⚙️ 与 Function App 集成示例

1. 启用 Function App 的系统托管身份  
2. 在 Key Vault 中配置访问授权：
   - 若为访问策略模式：添加身份并赋予 “Get Secret” 权限  
   - 若为 RBAC 模式：将身份添加到订阅或 Vault 上的“Key Vault Secrets User”角色
3. 在 Function App 的应用设置中使用以下格式访问 Secret：

```env
@Microsoft.KeyVault(SecretUri=https://<vault-name>.vault.azure.net/secrets/<secret-name>/)
```
---

## 🌐 Azure、AWS、GCP 关于 Key Vault 相关功能的对比

| 维度           | Azure Key Vault                                 | AWS Secrets Manager & KMS & Certificate Manager | GCP Secret Manager & KMS & Certificate Manager   |
|----------------|-------------------------------------------------|------------------------------------------------|--------------------------------------------------|
| 密钥管理       | 支持对称/非对称密钥，集成 Azure 加密服务        | KMS 支持对称/非对称密钥，集成 AWS 加密服务     | KMS 支持对称/非对称密钥，集成 GCP 加密服务       |
| 机密管理       | 支持存储字符串类型机密（如密码、API Key）       | Secrets Manager 支持存储字符串机密             | Secret Manager 支持存储字符串机密                |
| 证书管理       | 支持 TLS/SSL 证书存储、自动轮换、CA 集成         | Certificate Manager 支持证书存储与自动续期      | Certificate Manager 支持证书存储与自动续期        |
| 访问控制       | RBAC、访问策略、托管身份（Managed Identity）     | IAM Policy、资源策略、IAM 角色                  | IAM Policy、服务账号、细粒度权限                  |
| 审计与日志     | 集成 Log Analytics、Event Hub、Activity Log      | CloudTrail、CloudWatch Logs                     | Audit Logs、Cloud Monitoring                      |
| 自动轮换       | 支持机密/证书自动轮换                          | 支持机密/证书自动轮换                          | 支持机密/证书自动轮换                            |
| 与云服务集成   | 原生集成 VM、App Service、Function、AKS 等       | 原生集成 EC2、Lambda、RDS、ECS 等               | 原生集成 GCE、Cloud Run、GKE、Cloud SQL 等        |
| API/CLI 支持   | REST API、CLI、PowerShell、SDK                   | CLI、SDK、REST API                              | CLI、SDK、REST API                                |
| 多区域冗余     | 支持（标准/高级层）                             | 支持（KMS 多区域密钥、Secrets Manager 多区域）   | 支持（KMS/Secret Manager 多区域）                 |
| 费用模式       | 按操作/存储/高级功能计费                        | 按存储/调用/轮换计费                            | 按存储/调用/轮换计费                              |

**总结：**
- 三大云厂商均提供密钥、机密、证书的集中管理、自动轮换、细粒度权限和审计能力。
- Azure Key Vault 强调与托管身份、RBAC、云服务原生集成，AWS/GCP 以 IAM/服务账号为核心，集成能力同样完善。
- 证书管理、自动轮换、多区域冗余等能力均支持，建议结合安全合规和平台生态选择最佳方案。
---

## 🧪 小测验复盘（得分：13 / 20）

### 问题 1：Key Vault 功能
✅ A, C, D  
❌ B：不是用来加密磁盘，而是密钥的管理来源

### 问题 2：托管身份特性
✅ A, B  
❌ C：MI 没有密码  
❌ D：不能直接操作 Azure AD 用户权限

### 问题 3：Function App 授权方式
✅ A, B, C  
❌ D：不建议开放公共访问，推荐 Private Endpoint + MI

### 问题 4：RBAC 与访问策略比较
✅ A, B, D  
❌ C：两者不可同时启用

### 问题 5：支持原生集成 Key Vault 的服务
✅ A, B  
❌ C, D：AKS 与 Azure DevOps 也可通过 CSI 或任务变量组使用 Vault

---

## 🔜 下一步预告 - Day 45 模块

| 模块                     | 内容说明                             |
|--------------------------|--------------------------------------|
| Azure Storage 生命周期管理 | 自动删除/移动 Blob、冷热分层等配置   |
| 与 Function 联动         | 上传触发器、清理日志脚本示例         |
| 文件共享权限控制         | Azure Files + AD DS / RBAC 权限整合   |
