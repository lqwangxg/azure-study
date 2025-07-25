# 📘 AZ-500 Day 19 模拟题与知识库

## 📚 知识点总结

### 🔐 Microsoft Defender 家族核心产品

#### Defender for Identity
- 针对本地 Active Directory 进行监控
- 检测异常登录、Pass‑the‑Hash、横向移动、Kerberoasting 等
- 通过在域控制器上部署传感器工作

#### Defender for Endpoint
- 端点保护与 EDR 功能
- 包括漏洞检测、攻击面减小策略、自动隔离、事件调查
- 与 Intune 集成，实现设备合规性控制与条件访问

#### Defender for Cloud Apps（MCAS）
- SaaS 应用访问控制和下载保护
- OAuth 客户端管理、Shadow IT 发现与行为分析
- 与多种 SaaS 服务（Microsoft 365、Salesforce、Dropbox）集成

#### Microsoft Sentinel
- 云原生 SIEM + SOAR 平台
- 日志聚合、威胁检测、行为分析、Playbook 自动响应
- 支持 KQL 查询及跨平台数据接入

#### Microsoft Purview 审计日志
- 捕获 Office 365 用户操作，如文件操作、PowerShell 执行、邮箱访问等
- 支持合规审计与司法取证

---

## 🧪 模拟题：题干、你的答案、正确答案与解析

### 1. Defender for Identity 主要作用是什么？
**题干**：Microsoft Defender for Identity 的主要作用是？  
**选项**：  
A. 保护 SaaS 应用程序  
B. 检测本地 AD 中的可疑行为✅  
C. 加密 Azure Blob 存储  
D. 替代 Azure Firewall  

**解析**：Defender for Identity 专注监控本地 Active Directory，识别异常行为如横向移动、凭据滥用等。

---

### 2. 哪些是 Defender for Endpoint 提供的功能？（多选）
**题干**：关于 Defender for Endpoint 的功能，以下哪些是支持的？  
**选项**：  
A. 自动隔离受感染设备✅  
B. 实时数据备份  
C. 攻击面减小规则✅  
D. 设备漏洞管理 ✅  
**解析**：Defender for Endpoint 支持攻击面减小、端点隔离和设备漏洞管理，不提供实时数据备份功能。

---

### 3. Microsoft Defender for Cloud Apps 能实现哪些操作？（多选）
**题干**：Microsoft Defender for Cloud Apps 能够实现以下哪些操作？  
**选项**：  
A. 阻止未授权 OAuth 应用接入✅  
B. 探测用户异常位置登录✅  
C. 审计 Microsoft Teams 文件传输✅  
D. 配置 Azure VM 的 NSG  
**解析**：MCAS 支持 OAuth 应用控制、异常行为检测、Teams 文件活动审计，但不用于配置 Azure NSG。

---

### 4. Microsoft Sentinel 的作用是什么？
**题干**：Microsoft Sentinel 的作用是？  
**选项**：  
A. 统一日志收集与威胁检测✅  
B. 执行终端隔离操作  
C. 自动配置 Defender 策略  
D. 分析来自多种平台的数据✅  
**解析**：Sentinel 是一个 SIEM + SOAR 平台，负责多平台日志收集和威胁检测分析，不直接执行终端隔离。

---

### 5. 哪些行为可以被 Microsoft Purview 审计日志捕捉？（多选）
**题干**：下列哪些行为可以通过 Microsoft Purview 审计日志捕捉到？  
**选项**：  
A. 用户下载 OneDrive 文件✅  
B. Teams 聊天记录实时同步  
C. 邮件读取行为✅  
D. Exchange Online 命令执行记录✅  
**解析**：Purview 审计日志可以记录文件下载、邮件读取和管理员命令执行等操作，聊天同步属于日常通信，不会单独审计。

---

### 6. Defender for Endpoint 与 Intune 联合使用的功能有哪些？（多选）
**题干**：Defender for Endpoint 的哪些功能与 Intune 联合使用？  
**选项**：  
A. 设备合规性评估✅  
B. 漏洞修复调度✅  
C. 条件访问策略生效✅  
D. 应用更新补丁部署  
**解析**：Defender for Endpoint 可与 Intune 联动实现合规性控制、漏洞管理及条件访问触发，但不负责应用更新分发。

---

### 7. Sentinel 的哪项功能属于 SOAR？
**题干**：Microsoft Sentinel 的哪项功能属于 SOAR？  
**选项**：  
A. 使用 Playbook 自动封禁 IP✅  
B. 配置 WAF 规则集  
C. 自动升级 VM 大小  
D. 设置多区域可用性集  

**解析**：Playbook 是 Sentinel 的自动响应机制，能执行封禁、通知、工单创建等操作，属于 SOAR 范畴。

---

### 8. Defender for Cloud Apps 最佳集成服务包括哪些？（多选）
**题干**：Defender for Cloud Apps 与哪些服务集成效果最佳？  
**选项**：  
A. Microsoft 365✅  
B. Salesforce✅  
C. Dropbox✅  
D. Azure Bastion  

**解析**：MCAS 可与多种 SaaS 平台集成（如 Microsoft 365、Salesforce、Dropbox），但不与 Azure Bastion 集成。

---

### 9. 哪些不属于 Defender for Identity 的能力？（多选）
**题干**：以下哪些不属于 Defender for Identity 的能力？  
**选项**：  
A. 检测传感器收集的域控活动  
B. 防止用户重置 MFA✅  
C. 检测横向移动行为  
D. 警报 AD 用户异常登录  
**解析**：Defender for Identity 不负责管理 MFA，也不控制其重置流程，其核心是检测横向移动、异常登录等 AD 安全行为。
### 🧠 Defender for Identity 简介：
- 主要功能：保护本地 AD 环境，检测身份相关攻击行为。
- 集成组件：与 Microsoft 365 Defender 平台集成，提供统一威胁视图。
#### 核心能力：
- Reconnaissance（侦察）
- Credential Theft（凭据盗窃）
- Lateral Movement（横向移动）
- Domain Dominance（域控制权）

---

### 10. 哪些服务负责对终端或账户进行实时响应？（多选）
**题干**：以下哪些服务负责对终端或账户进行实时响应？  
**选项**：  
A. Defender for Endpoint✅  
B. Sentinel  
C. Defender for Identity✅  
D. MCAS✅  

**解析**：
- Defender for Endpoint 可以隔离设备；
- Defender for Identity 可禁用账户；
- MCAS 可终止会话，
- Sentinel 则偏向日志分析，主要用于日志分析和流程自动化，而非主动响应操作。