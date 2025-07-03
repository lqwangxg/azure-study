
# AZ-104 学习笔记 - Day 18

## ☁️ 今日主题：Azure Storage 架构与性能优化

---

## 📦 存储账户类型

| 类型 | 特点 |
|------|------|
| General Purpose v2 | 推荐类型，支持 Blob、File、Queue、Table |
| Blob Storage        | 仅限 Blob，对访问层控制更精细 |

---

## 🔄 冗余策略（Replication Options）

| 冗余方式 | 描述 | 区域内/跨区域 | 可读性 |
|----------|------|---------------|--------|
| LRS      | 本地冗余（单区域三副本） | 区域内 | 否 |
| ZRS      | 区域内多可用区同步 | 区域内 | 是 |
| GRS      | 区域冗余（异地备份） | 跨区域 | 否 |
| RA-GRS   | GRS + 备份区只读 | 跨区域 | 是 |
| GZRS     | ZRS + 异地备份 | 跨 + 区域 | 否 |
| RA-GZRS  | GZRS + 异地备份区只读 | 跨 + 区域 | 是 |

---

## 🥶 Blob 存储访问层（Access Tiers）

| 层级 | 特点 | 适用场景 |
|------|------|----------|
| 热   | 频繁读写 | 活跃文件，如网站资源、图片等 |
| 冷   | 不常访问，费用低 | 日志、备份 |
| 存档 | 极少访问，需要 Rehydrate 解冻 | 法务数据、归档、长期保留数据 |

> 可在创建或事后变更访问层，按 Blob 级或容器级设置。

---

## 🔐 存储安全访问机制

### ✅ RBAC 控制
- 通过角色（如 Storage Blob Data Contributor）管理用户/服务对 Blob 资源的权限

### ✅ SAS（共享访问签名）
- 生成带有时间限制、操作限制的临时 URL
- 可控制权限（读/写/列出）、起止时间、IP 范围
- 常用于外部系统上传或下载

> SAS 不依赖 Azure AD，可用于匿名访问控制。

---

## 🌐 网络安全控制

| 功能 | 说明 |
|------|------|
| 网络访问规则 | 限定特定 IP、VNet |
| 私有终结点（Private Endpoint） | 通过专属 IP 接入，屏蔽公网 |
| 与 NSG 配合 | 控制进出子网/VM 的访问权限 |

---

## 🔒 加密机制（Encryption）

| 类型 | 描述 |
|------|------|
| SSE（平台加密） | 默认启用，由 Azure 管理密钥 |
| CMK（客户托管密钥） | 存储于 Key Vault，自行轮换管理 |
| 双重加密 | CMK + SSE 同时作用 |

> 平台加密不可禁用。

---

## 🌐 Azure、AWS、GCP 存储服务对比

| 维度           | Azure Storage                                 | AWS Storage                                 | GCP Storage                                 |
|----------------|----------------------------------------------|---------------------------------------------|---------------------------------------------|
| 对象存储       | Blob Storage（Block/Append/Page Blob）        | S3（Standard/IA/Glacier）                   | Cloud Storage（Standard/Nearline/Coldline/Archive） |
| 文件存储       | Azure Files（SMB/NFS）                        | EFS（NFS）、FSx（SMB/NFS）                  | Filestore（NFS）                            |
| 块存储         | Azure Disk（托管磁盘）                        | EBS（Elastic Block Store）                  | Persistent Disk                             |
| 冗余选项       | LRS/ZRS/GRS/RA-GRS/GZRS/RA-GZRS               | S3 Standard、S3-IA、S3 One Zone、CRR、Glacier | 多区域、单区域、近线、冷线、归档            |
| 访问层         | 热/冷/存档                                    | Standard/IA/Glacier                         | Standard/Nearline/Coldline/Archive          |
| 授权机制       | RBAC、SAS、存储账户密钥、Azure AD             | IAM Policy、Access Key、Bucket Policy、临时凭证 | IAM、签名URL、服务账号                      |
| 加密           | 默认 SSE，支持 CMK（Key Vault），传输加密      | 默认 SSE，支持 KMS/BYOK，传输加密           | 默认 SSE，支持 CMEK（KMS），传输加密         |
| 生命周期管理   | 支持自动分层、归档、删除                      | 支持生命周期规则、自动分层、归档             | 支持生命周期规则、自动分层、归档             |
| 私有网络接入   | Private Endpoint（专属 IP）                   | VPC Endpoint（Gateway/Interface）           | Private Google Access、VPC Service Controls  |
| 快照/备份      | Blob/Files 支持快照，定期备份                 | S3 Versioning、EBS/EFS 快照、备份服务        | Object Versioning、磁盘快照                  |
| 文件共享集成   | 支持 AD DS/本地 AD 集成                       | 支持 AD 集成（FSx for Windows）             | 支持 AD 集成（Filestore Enterprise）         |
| 管理工具       | Azure Portal、CLI、PowerShell、REST API        | AWS Console、CLI、SDK、CloudFormation       | GCP Console、gcloud、API、Terraform          |

**总结：**
- 三大云厂商均提供对象、文件、块存储，支持多种冗余、加密、生命周期管理和私有网络接入。
- 授权机制和加密能力均支持企业级集成与合规要求，细节和命名略有差异。
- 建议结合实际业务需求、性能、合规和成本选择合适的云存储服
---

## ✅ 小测验回顾（4 / 5）

### 1. 跨区域 + 可读备份访问：✅ D. RA-GRS

### 2. 多可用区写入同步：✅ B. ZRS

### 3. Archive Blob 读取前操作：✅ C. 需解冻（Rehydrate）

### 4. SAS 正确用途：✅ A. 限制操作和时间；✅ C. 共享机制；✅ D. 粒度细于 RBAC

### 5. 加密机制：
- ✅ A. 默认开启平台加密
- ❌ B. 平台加密密钥不是存于 Key Vault
- ✅ C. 支持客户托管密钥（CMK）
- ✅ D. 默认加密不可关闭

---

## 🧠 今日记忆重点

- General Purpose v2 是默认推荐类型，支持所有存储服务
- 冗余策略选择影响性能/可用性/成本：ZRS > LRS，RA-GRS > GRS
- 存档层需解冻后读取，适用于冷数据
- SAS 提供更细粒度访问控制，不依赖 Azure AD
- 加密机制默认启用，CMK 提供自控选项，SSE 不可禁用

---

## 📅 下一步预告 - Day 19

| 模块                         | 内容                             |
|------------------------------|----------------------------------|
| Blob 生命周期策略            | 自动转档 / 删除老旧数据          |
| 成本控制最佳实践              | 冷/存档层优化、日志/备份存储分层 |
| 生命周期规则语法与限制        | JSON 配置结构与时间范围单位     |
