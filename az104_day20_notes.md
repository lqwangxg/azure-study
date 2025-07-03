
# AZ-104 学习笔记 - Day 20

## 📁 今日主题：Azure Files 与 Azure File Sync（混合文件架构）

---

## ☁️ Azure Files 简介

Azure Files 是一种完全托管的文件共享服务，支持以下协议：

- ✅ SMB（Server Message Block）
- ✅ NFS（Network File System）
- ❌ 不支持 FTP、HTTP 等协议访问

可通过挂载方式供 Azure VM、本地服务器、容器访问，适用于：
- 替代传统文件服务器
- 文件共享、日志存储、配置文件持久化等场景

---

## 🔐 权限控制与身份集成

| 控制类型 | 说明 |
|----------|------|
| RBAC     | 控制谁能挂载或管理共享资源（如读写权限） |
| NTFS ACL | 控制文件夹/文件级访问权限（细粒度），需启用 Active Directory 集成 |
| 身份绑定 | 存储账户需加入域（AD DS）才能使用 NTFS ACL 生效 |

> NTFS 权限控制仅在启用 **身份集成 + SMB 协议** 下使用。

---

## 🔁 Azure File Sync

用于在本地服务器与 Azure Files 间进行**双向同步**，支持：

- 多服务器同步到同一共享（Cloud Endpoint）
- 保留本地热点数据（Cloud Tiering）
- 快速灾备与远程访问
- 跨区域文件集中管理

### 架构组件

| 组件 | 作用 |
|------|------|
| Storage Sync Service | 管理中心 |
| Registered Server | 安装 agent 的本地服务器 |
| Sync Group | 包含 cloud endpoint 与多个 server endpoint |
| Cloud Tiering | 控制本地缓存比例（如只保留 20% 热点数据） |

---

## 💰 成本与性能建议

| 项目 | 说明 |
|------|------|
| Azure Files 存储费 | 按容量计费（标准 / Premium） |
| File Sync | 同步操作可能产生 API 调用费用 |
| Premium Files | 提供高 IOPS、低延迟，适合高性能需求 |
| 带宽费用 | 访问跨区域、外部下载会计入出站流量费用 |

---

## ⚠️ 注意事项

- 访问共享需开启身份集成，否则 NTFS 权限无效
- File Sync 不提供数据压缩功能
- 文件共享支持设置配额、快照备份
- 文件存储可与备份中心、日志采集工具整合

---

## 🌐 Azure、AWS、GCP 文件共享服务对比

| 维度           | Azure Files                                 | AWS EFS / FSx                              | GCP Filestore                              |
|----------------|---------------------------------------------|--------------------------------------------|--------------------------------------------|
| 服务类型       | 托管 SMB/NFS 文件共享                       | EFS（NFS）、FSx for Windows（SMB）、FSx for Lustre | 托管 NFS 文件共享                          |
| 协议支持       | SMB、NFS                                    | NFS（EFS）、SMB（FSx for Windows）、Lustre（FSx for Lustre） | NFS（v3/v4）                               |
| 身份集成       | 支持 AD DS、本地 AD、Azure AD DS            | 支持 AD（FSx for Windows）、IAM（EFS）      | 支持 AD（Filestore Enterprise）、IAM        |
| 访问控制       | RBAC、NTFS ACL（需身份集成）、存储账户密钥   | POSIX 权限（EFS）、NTFS ACL（FSx）、IAM     | POSIX 权限、AD 集成、IAM                   |
| 多区域支持     | 支持跨区域同步（File Sync）                  | EFS/FSx 跨 AZ，FSx for Windows 支持多区域复制 | 支持多区域部署（部分版本）                  |
| 快照/备份      | 支持快照、与 Azure Backup 集成               | 支持快照、AWS Backup 集成                  | 支持快照、与 GCP Backup 集成                |
| 性能层级       | 标准、Premium                               | 标准、性能型、FSx 提供多种性能选项          | 标准、性能型                               |
| 自动分层/缓存  | Cloud Tiering（File Sync）                   | EFS IA（自动分层）、FSx 支持缓存            | 支持缓存（部分版本）                        |
| 计费模式       | 按容量、性能层、操作、出站流量计费           | 按容量、性能层、操作、出站流量计费          | 按容量、性能层、操作、出站流量计费          |
| 管理工具       | Portal、CLI、PowerShell、REST API            | Console、CLI、SDK、CloudFormation           | Console、gcloud、API、Terraform             |

**总结：**
- 三大云厂商均提供托管文件共享服务，支持主流协议（SMB/NFS）、身份集成和快照备份。
- Azure Files 通过 File Sync 支持本地与云端双向同步和分层缓存，AWS EFS/FSx 提供多协议和自动分层，GCP Filestore 适合高性能 NFS 场景。
- 权限控制、性能层级和备份能力均较为完善，建议结合实际业务需求和平台特性选择合适的文件共享服
---

## ✅ 小测验回顾（5 / 5）

### 1. Azure Files 支持哪些协议？
- ✅ A. SMB
- ✅ B. NFS

### 2. 使用 AD 凭证访问共享前提？
- ✅ B. 启用身份集成 + 存储账户域加入

### 3. Azure File Sync 不支持什么？
- ✅ B. 自动压缩文件

### 4. 启用文件层访问控制？
- ✅ C. NTFS ACL（依赖 AD 集成）

### 5. 成本与性能建议？
- ✅ B. Premium 提供高 IOPS
- ✅ C. 同步会产生 API 调用费用

---

## 📅 下一步预告 - Day 21

| 模块 | 内容 |
|------|------|
| Azure Files 快照与恢复机制 | 文件共享级快照管理 |
| 存储备份策略与自动化 | Azure Backup、Recovery Vault |
| 文件服务常见问题分析 | 挂载失败、权限冲突、性能瓶颈等 |

