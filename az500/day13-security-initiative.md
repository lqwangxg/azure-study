# 📘 AZ-500 Day 13：题目 + 答案解析 + 错题讲解（Markdown 完整版）

## 🧠 今日知识点回顾

### 🔐 Microsoft Defender for Cloud 的核心功能
- 安全分数（Secure Score）：展示整体安全合规状态。
- 安全建议（Recommendations）：提出改善建议，如启用加密或 HTTPS。
- 安全倡议（Security Initiative）：Azure Policy 的组合，用于合规性评估。
- 威胁防护（Threat Protection）：针对资源提供高级威胁防护。

### 💾 Azure Disk Encryption
- 使用 Key Vault 来托管磁盘加密密钥（默认选项）。
- 支持双重加密和客户管理的密钥（CMK）。

### 🧱 启用 Secure Boot 与 VBS（虚拟化安全）
- Secure Boot 防止恶意引导加载程序运行。
- VBS（Virtualization-Based Security）加强内存完整性。

### 🛡️ Defender 支持的计划（示例）
- Defender for Servers
- Defender for SQL
- Defender for DNS
- Defender for Containers

（**Defender for Billing 不存在**）

### 🏢 安全策略的跨订阅应用
- 可使用 Azure **Blueprint** 将策略组合部署至多个订阅。
- 也可借助 Azure Policy + Initiative 集合管理。

---

## ✅ 第 1 题

**题目：**  
Microsoft Defender for Cloud 中的 "Secure Score" 功能的主要作用是什么？

**选项：**  
A. 提供资源的成本分析  
B. 测量 VM 的性能指标  
C. 评估当前资源的安全配置并提出改进建议  
D. 管理资源组权限配置  

**你的答案：** C  
**正确答案：** C  
**解析：** Secure Score 是用来衡量你环境中资源的安全状况，并根据最佳实践提供优化建议。它不分析成本或性能。

---

## ✅ 第 2 题

**题目：**  
以下哪个功能可用于防止在 VM 上加载未经授权的操作系统？

**选项：**  
A. 网络安全组 (NSG)  
B. 安全启动 (Secure Boot)  
C. Azure Bastion  
D. 网络隔离  

**你的答案：** B  
**正确答案：** B  
**解析：** Secure Boot 是一种安全标准，防止加载未经授权的内核或操作系统映像，保护引导过程完整性。

---

## ✅ 第 3 题

**题目：**  
Microsoft Defender for Cloud 推荐启用哪个功能来确保 Web 应用程序安全地传输数据？

**选项：**  
A. HTTPS only  
B. RDP over HTTP  
C. Web Application Proxy  
D. 强制 TLS 1.1  

**你的答案：** A  
**正确答案：** A  
**解析：** “HTTPS only” 确保客户端与 Web 应用之间的数据通过加密连接传输，是提升安全性的重要建议。

---

## ❌ 第 4 题

**题目：**  
Security Initiative（安全倡议）在 Defender for Cloud 中的角色是什么？

**选项：**  
A. 自动修复资源安全漏洞的工具  
B. 一组用于安全合规评估的策略集合  
C. 高级监控日志的集中平台  
D. 提供虚拟机补丁管理的解决方案  

**你的答案：** A  
**正确答案：** B  
**解析：** Security Initiative 是由多条 Azure Policy 组成的策略集合，目的是用于评估环境的安全合规性，不是修复工具。

---

## ✅ 第 5 题

**题目：**  
你想在 Azure 虚拟机中启用磁盘加密，并使用客户自有密钥进行加密，以下哪项是必要组件？

**选项：**  
A. Azure Monitor  
B. Azure Key Vault  
C. Azure Bastion  
D. Azure Policy  

**你的答案：** B  
**正确答案：** B  
**解析：** 使用 Azure Disk Encryption 时，必须用到 Azure Key Vault 来保存加密密钥，尤其是在使用客户管理密钥（CMK）时。

---

## ✅ 第 6 题

**题目：**  
以下哪些是 Microsoft Defender for Cloud 中支持的资源类型？（多选）

**选项：**  
A. Azure SQL  
B. Azure DNS  
C. Azure 计费服务  

**你的答案：** A, B, C  
**正确答案：** A, B  
**解析：** Defender 支持 SQL、Storage、DNS、Containers 等资源，但目前没有“Defender for Billing”这一选项。

---

## ✅ 第 7 题

**题目：**  
你希望限制用户只能在特定 Azure 区域部署资源，应使用哪项功能？

**选项：**  
A. Azure RBAC  
B. Azure Monitor  
C. Azure Policy  
D. Azure Sentinel  

**你的答案：** C  
**正确答案：** C  
**解析：** Azure Policy 可以用于限制资源部署区域、SKU 类型等，是强制合规的首选工具。

---

## ✅ 第 8 题

**题目：**  
你想确保 VM 的启动配置文件没有被篡改，应启用以下哪项功能？（多选）

**选项：**  
A. Virtualization-Based Security (VBS)  
B. Secure Boot  
C. Just-In-Time VM Access  
D. Azure Firewall  

**你的答案：** B  
**正确答案：** A, B  
**解析：** Secure Boot 和 VBS 共同提高 VM 启动与内存区域的安全性。VBS 会启用内存完整性保护。

---

## ✅ 第 9 题

**题目：**  
你正在为多订阅环境制定统一安全策略，应使用下列哪项工具？

**选项：**  
A. Azure Monitor  
B. Azure Firewall Manager  
C. Azure Policy Initiative  
D. Network Watcher  

**你的答案：** C  
**正确答案：** C  
**解析：** Azure Policy Initiative 可以将多个 Policy 打包组合并应用到多个订阅，实现跨订阅的一致性安全策略控制。

---

## ✅ 第 10 题

**题目：**  
以下哪些功能可以用于管理和部署多个订阅的安全策略？（多选）

**选项：**  
A. Azure Policy  
B. Management Groups  
C. Azure Monitor  
D. Azure Security Center Initiative  

**你的答案：** A  
**正确答案：** A, B  
**解析：** Policy 和 Management Groups 配合使用，可以跨多个订阅统一部署策略与访问控制。

---