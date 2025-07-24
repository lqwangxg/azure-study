# 📘 AZ-500 Day 17 - 数据加密与密钥管理

## ✅ 今日重点知识点总结

### 🔐 数据加密方式
- **静态数据加密（At-Rest Encryption）**
  - 使用 Azure Storage Service Encryption (SSE)
  - 加密服务支持 Blob、Disk、File、Queue 等
  - 加密算法：AES-256（对称加密）

- **传输中加密（In-Transit Encryption）**
  - 默认使用 **TLS 1.2 / 1.3**
  - 保护客户端与 Azure 服务之间的数据传输

### 🔐 Azure Disk Encryption (ADE)
- 用于加密 IaaS 虚拟机的 OS 和数据磁盘
- 支持 Windows（BitLocker）和 Linux（DM-Crypt）
- 与 Azure Key Vault 集成管理密钥

### 🔐 密钥管理模式
- **Microsoft-Managed Keys (MMK)**
  - 由 Microsoft 自动创建、轮换、管理

- **Customer-Managed Keys (CMK)**
  - 存储于 Azure Key Vault
  - 支持手动轮换或自定义自动轮换策略

### 🔐 密钥生命周期管理
- 包括：生成、使用、备份、轮换、吊销、销毁
- 轮换策略降低泄露风险
- Key Vault 支持密钥版本控制与访问策略

---

## ❌ 错题集回顾

### ❌ 第 7 题  
**题目：** 哪种加密算法通常用于对称加密存储中的数据？  
- **你的答案：** A. RSA  
- **正确答案：** B. AES  
- **解析：**  
  - RSA 属于非对称加密算法，适用于密钥交换与身份验证  
  - AES（Advanced Encryption Standard）是目前广泛用于存储加密的对称加密算法

---

### ❌ 第 8 题  
**题目：** Azure 存储加密支持哪些密钥管理模式？  
- **你的答案：** A、B、C、D  
- **正确答案：** A、B  
- **解析：**  
  - Azure 支持 **Microsoft-managed keys** 和 **Customer-managed keys**  
  - 本地密钥和第三方 HSM 不属于 Azure 原生支持范围，必须通过间接方式（如 BYOK）

---

