# AZ-104 学习笔记 - Day 12

## 🎯 今日主题：Azure 存储服务

---

## 📦 存储账户类型

### 1. 性能层级

| 类型 | 用途 | 特点 |
|------|------|------|
| Standard | 大多数应用 | 使用 HDD 或通用 SSD（便宜） |
| Premium | 低延迟、高 IOPS | 使用高性能 SSD，适用于数据库、日志等高频访问场景 |

### 2. 冗余选项（复制方式）

| 类型 | 冗余方式 | 用途 |
|------|----------|------|
| LRS | 本地冗余（同区域3副本） | 默认选项，最低成本 |
| ZRS | 区域内多数据中心冗余 | 提升可用性，支持读写 |
| GRS | 跨区域冗余 | 主区域写，备份于次区域 |
| RA-GRS | 跨区域 + 可读访问 | 可灾备读取 |
| GZRS / RA-GZRS | ZRS + GRS 组合 | 提供最高级别的数据可用性与灾难恢复能力 |

---

## 🧱 Blob 存储结构

### ⛱ 容器（Container）
- 用于组织 Blob 文件
- 权限分级：私有 / 公有 Blob / 公有容器

### 🧊 Blob 类型

| 类型 | 用途 |
|------|------|
| Block Blob | 最常用，适合视频、文档等 |
| Append Blob | 日志类，支持追加写入 |
| Page Blob | 随机 IO，用于 Azure Disk（VHD 文件） |

### 🚦 访问层（Access Tier）

| 层级 | 描述 |
|------|------|
| Hot | 频繁访问，访问便宜，存储贵 |
| Cool | 不常访问，存储便宜 |
| Archive | 存储最便宜，访问需“解冻”，延迟高（小时级） |

---

## 🔐 存储安全控制方式

### 1. 授权机制

| 方式 | 描述 | 场景 |
|------|------|------|
| 存储账户密钥 | root 权限 | 后台管理程序、容器级别操作 |
| SAS | 共享访问签名，支持时间/权限限制 | 外部上传、下载链接 |
| RBAC + Azure AD | 企业级身份授权 | 精细化权限控制与审计 |

### 2. 加密（Encryption）

- **默认开启服务端加密（SSE）**
- 使用平台托管密钥（默认）
- 可配置 BYOK（Bring Your Own Key）来自管密钥
- 支持加密数据传输（HTTPS）

---

## 🗂 Azure Files 简介

- 协议：SMB（Windows） / NFS（Linux）
- 可映射为本地磁盘挂载
- 可与 Azure AD DS / On-Prem AD 集成，提供基于身份的权限控制
- 支持快照功能，便于恢复历史文件版本

---

## ✅ 小测验回顾（4 / 5）

### 1. 哪种冗余选项提供跨区域的读访问能力？
- ❌ 你的答案：C（GRS）  
- ✅ 正确答案：D（RA-GRS）  
- ✅ 解析：GRS 仅备份，RA-GRS 可读辅助区域数据。

### 2. 哪种类型的 Blob 最适合日志写入？
- ✅ 正确答案：C（Append Blob）

### 3. 存储账户默认启用的加密机制？
- ✅ 正确答案：B（SSE）、C（平台托管密钥）

### 4. 提供临时 HTTPS 下载链接？
- ✅ 正确答案：B（SAS）

### 5. 关于 Archive 层的正确描述？
- ✅ 正确答案：A、B

---

## 🧠 总结记忆要点

- 默认加密 = SSE + 平台托管密钥
- Archive 需解冻，适合归档
- Append Blob 适合日志写入
- Azure Files 支持 SMB/NFS + AD 验证
- RA-GRS 才能跨区域读访问

---

## 🌐 Azure 与 AWS 存储资源对比

| 维度           | Azure Storage                                 | AWS Storage                                 |
|----------------|----------------------------------------------|---------------------------------------------|
| 对象存储       | Blob Storage（Block/Append/Page Blob）        | S3（Standard/IA/Glacier）                   |
| 文件存储       | Azure Files（SMB/NFS）                        | EFS（NFS）、FSx（SMB/NFS）                  |
| 块存储         | Azure Disk（Page Blob 实现）                  | EBS（Elastic Block Store）                  |
| 冗余选项       | LRS/ZRS/GRS/RA-GRS/GZRS/RA-GZRS               | S3 Standard、S3-IA、S3 One Zone、Cross-Region Replication、Glacier |
| 访问层         | Hot/Cool/Archive                              | S3 Standard/IA/Glacier Deep Archive         |
| 授权机制       | 存储账户密钥、SAS、RBAC、Azure AD             | Access Key、IAM Policy、Bucket Policy、临时凭证 |
| 加密           | 默认 SSE，支持 BYOK，传输加密（HTTPS）         | 默认 SSE，支持 KMS/BYOK，传输加密（HTTPS）   |
| 快照/备份      | Blob/Files 支持快照，定期备份                 | S3 Versioning、EBS/EFS 快照、备份服务        |
| 文件共享集成   | 支持 AD DS/本地 AD 集成                       | 支持 AD 集成（FSx for Windows）             |
| 生命周期管理   | 支持自动分层、归档、删除                      | 支持生命周期规则、自动分层、归档             |

**总结：**
- Azure Blob ≈ AWS S3，均支持多层存储、生命周期管理、加密与访问控制。
- Azure Files 类似 AWS EFS/FSx，均支持文件共享和身份集成。
- 冗余和归档机制两者均丰富，命名和细节略有差异，建议结合实际业务需求选择。
- 授权和加密机制均支持企业级集成与合规要求。

---

## 📅 Day 13 预告：网络安全组与访问控制

| 模块 | 内容 |
|------|------|
| NSG | 入站/出站规则，优先级、默认规则 |
| ASG | 应用安全组 vs NSG 配合 |
| UDR | 路由控制：强制走防火墙/NAT |
| 小测验 | NSG 场景判断与规则配置题 |
