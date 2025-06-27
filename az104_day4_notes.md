
# AZ-104 学习笔记 - Day 4

## 🎯 学习目标
- 理解 Azure 存储账户的类型与冗余模型
- 掌握 Blob / File / Queue / Table 四种服务的使用场景
- 掌握授权模型（Shared Key、SAS、RBAC、防火墙）的应用
- 使用 Azure CLI 实际操作 Blob 存储
- 通过测验检测掌握情况

---

## 🗃️ 存储账户类型（Storage Account）

### 📦 存储账户类型
| 类型 | 用途 | 特点 |
|------|------|------|
| **Standard** | 常规使用 | 支持所有服务，较低成本 |
| **Premium** | 高性能场景 | 低延迟，适合数据库磁盘、实时分析等 |

---

### 🌍 冗余选项（复制模型）
| 类型 | 名称 | 描述 | 用途 |
|------|------|------|------|
| **LRS** | 本地冗余存储 | 区域内 3 份副本 | 成本低，适合基本容错 |
| **ZRS** | 区域冗余存储 | 跨 AZ 冗余 | 增强可用性，适合 SLA 高需求场景 |
| **GRS** | 跨区域冗余 | 主要 + 副本 Region | 异地灾备 |
| **GZRS** | 跨区域 + 多 AZ | GRS + ZRS，最强容错 | 关键应用系统推荐使用 |

---

## ☁️ 存储服务概览

| 服务 | 用途 | 特点 |
|------|------|------|
| **Blob** | 非结构化对象存储 | 支持 Block/Page/Append 类型 |
| **File** | SMB 文件共享 | 可挂载至本地或 Azure VM |
| **Queue** | 消息通信 | 适合微服务间异步处理 |
| **Table** | NoSQL 数据存储 | 键值型结构化数据，查询快速，扩展性强 |

---

## 🔐 授权与访问控制机制

| 机制 | 控制方式 | 场景 |
|------|----------|------|
| **Shared Key** | 全账户级权限，基于密钥 | 脚本/自动化场景 |
| **SAS（Shared Access Signature）** | URL 级访问控制，可限时/限权限 | 文件共享、短期授权 |
| **RBAC** | 基于角色的授权，依赖 Azure AD | 企业安全控制首选 |
| **防火墙/VNet 限制** | IP 或子网级访问限制 | 提高安全性，与其他机制联合使用效果更佳 |

---

## 🛠️ 实操命令示例（使用 Azure CLI）

```bash
# 登录并创建资源组
az login
az group create --name rg-day4 --location japaneast

# 创建存储账户
az storage account create   --name stday4example   --resource-group rg-day4   --location japaneast   --sku Standard_LRS

# 获取 Key 上传 Blob 文件
key=$(az storage account keys list --resource-group rg-day4 --account-name stday4example --query [0].value -o tsv)

az storage container create --account-name stday4example --name demo --account-key $key

az storage blob upload   --account-name stday4example   --account-key $key   --container-name demo   --name test.txt   --file ./test.txt   --overwrite
```

---

## 🧪 Day 4 小测验回顾（全对 🎉）

| 题号 | 正确答案 | 说明 |
|------|-----------|------|
| 1 | D | GZRS 提供最强的可用性和灾备能力 |
| 2 | D | Blob 最适合异步处理日志数据 |
| 3 | A, B, D | SAS 可控制时间、权限、来源 IP |
| 4 | C | 使用 RBAC 结合 Azure AD 控制权限是最佳实践 |
| 5 | C | Premium Storage 适合高 IO 场景，如数据库磁盘 |

---

## 📌 总结记忆重点

- 存储账户类型以 Standard 为主，Premium 针对性能
- GRS 和 GZRS 提供跨区域灾备
- Blob 是最常用对象存储，Queue 常用于微服务通信
- 授权优先推荐：**RBAC > SAS > Shared Key**

---

## 📅 下一步 - Day 5 预告

| 模块 | 内容 |
|------|------|
| 身份与访问控制 | Azure AD、Role、Scope、Assignment |
| 管理身份 | 系统分配 / 用户分配身份 |
| 实操 | 使用 Terraform 或 Portal 进行角色赋权 |
| 测验 | 权限边界与角色类型理解题 |
