# AZ-104 学习笔记 - Day 25

## ☁️ 今日主题：Azure 存储服务与访问控制策略

---

## 📦 Azure Storage 服务类型

| 类型             | 用途 |
|------------------|------|
| Blob Storage     | 非结构化数据（文档、图像、备份） |
| File Storage     | 提供 SMB 文件共享，适合虚拟机共享访问 |
| Queue Storage    | 消息队列，适用于异步通信 |
| Table Storage    | NoSQL 键值型数据库，轻量级存储 |

---

## ❄️ 生命周期管理

自动优化存储层级、降低成本：

| 策略               | 描述 |
|--------------------|------|
| 转换为冷存储       | 低频访问数据转移以节省费用 |
| 转换为归档存储     | 几乎不访问的数据归档（解冻访问） |
| 自动删除           | 例如 180 天后自动清除旧数据 |
| 支持前缀匹配/标签控制 | 可对特定路径/标签进行处理 |

---

## 🔐 加密策略（Encryption）

| 类型                           | 描述 |
|--------------------------------|------|
| SSE (服务端加密)               | 默认启用，自动由 Azure 管理密钥 |
| SSE + 客户密钥 (CMK)           | 通过 Azure Key Vault 提供密钥控制 |
| 客户端加密                     | 上传前自行加密，增加端到端安全性 |

---

## 🛡️ 安全控制策略

| 安全策略类型        | 功能说明 |
|---------------------|----------|
| SAS Token            | 有效期、权限、路径细粒度控制 |
| RBAC 授权            | 控制用户或服务身份的操作权限 |
| 防火墙 + 虚拟网络    | 限定 IP 或子网访问 |
| Private Endpoint     | 专线访问，仅可通过 VNet 访问 |
| 匿名访问（不推荐）   | 可访问公开容器，具安全风险 |

---

## 🌐 Azure、AWS、GCP 存储服务与访问控制策略对比

| 维度           | Azure Storage                                 | AWS Storage                                 | GCP Storage                                 |
|----------------|----------------------------------------------|---------------------------------------------|---------------------------------------------|
| 对象存储       | Blob Storage（Block/Append/Page Blob）        | S3（Standard/IA/Glacier）                   | Cloud Storage（Standard/Nearline/Coldline/Archive） |
| 文件存储       | Azure Files（SMB/NFS）                        | EFS（NFS）、FSx（SMB/NFS）                  | Filestore（NFS）                            |
| 块存储         | Azure Disk（托管磁盘）                        | EBS（Elastic Block Store）                  | Persistent Disk                             |
| 访问控制模型   | RBAC（基于角色）、存储账户密钥、SAS、Azure AD | IAM Policy、Bucket Policy、Access Key、临时凭证 | IAM Policy、签名URL、服务账号               |
| 细粒度授权     | 支持到容器/Blob 级别（RBAC/SAS）              | 支持到桶/对象级别（IAM/Policy）             | 支持到桶/对象级别（IAM/Policy）             |
| 身份集成       | 支持 Azure AD、AD DS、RBAC                    | 支持 IAM、AD（FSx for Windows）             | 支持 IAM、AD（Filestore Enterprise）         |
| 临时访问       | SAS（共享访问签名，带权限/时间限制）          | 预签名 URL、临时凭证（STS）                 | 签名 URL、临时凭证                          |
| 访问日志与审计 | 支持存储分析、Activity Log、Monitor            | S3 Access Log、CloudTrail、CloudWatch       | Audit Logs、存储访问日志                    |
| 加密           | 默认 SSE，支持 CMK（Key Vault）、传输加密      | 默认 SSE，支持 KMS/BYOK，传输加密           | 默认 SSE，支持 CMEK（KMS），传输加密         |

**总结：**
- 三大云厂商均支持对象、文件、块存储，访问控制均可细粒度到桶/容器/
---

## ✅ 小测验回顾（4.5 / 5）

### 1. 适用于虚拟机间共享的存储类型？

- ✅ B. File Storage

### 2. 生命周期策略支持？

- ✅ A. 30 天未访问转冷存储  
- ✅ B. 上传后 180 天归档  
- ✅ C. 90 天后自动删除

### 3. 加密方式？

- ✅ A. SSE（默认）  
- ✅ B. CMK（Key Vault）  
- ✅ D. 客户端加密

### 4. 限定虚拟网络访问的方式？

- ✅ B. 配置防火墙并启用 VNet 规则  
- ✅ D. 使用 Private Endpoint  
（你漏选 D）

### 5. 支持细粒度访问控制的机制？

- ✅ A. SAS Token  
- ✅ B. Storage Blob Data Reader

---

## 📅 下一步预告 - Day 26

| 模块 | 内容 |
|------|------|
| Azure 文件恢复与备份 | Recovery Vault、Snapshot、Soft Delete |
| 监控与诊断日志       | 存储监控、访问日志、威胁侦测 |
| 存储性能与扩展       | 存储账户限制与性能指标 |
