# 🌐 AZ-500 Day 7 学习笔记：Identity Protection 与 条件访问策略

---

## 📚 今日重点知识点整理

### 🔐 Identity Protection
Azure AD Identity Protection 是基于风险的自动身份防护机制。

#### 核心功能：
- 检测用户与登录风险：
  - 登录风险（匿名 IP、TOR、异常位置等）
  - 用户风险（凭据外泄、非正常行为模式）
- 自动响应：强制 MFA、阻止访问、重置密码等
- 集成 Microsoft 威胁情报与机器学习分析

#### 内建策略（Built-in Policies）：
1. **用户风险策略（User Risk Policy）**  
   高风险用户登录时强制重置密码

2. **登录风险策略（Sign-in Risk Policy）**  
   高风险登录尝试时强制 MFA 或阻止访问

3. **MFA 注册策略（MFA Registration Policy）**  
   要求用户在首次登录时注册 MFA

---

### 📏 条件访问（Conditional Access）

条件访问是一种基于策略的访问控制系统，用于在特定条件下允许或拒绝访问资源。

#### 策略结构：
- **分配对象（Assignment）**
  - 用户/组
  - 云应用
  - 条件（位置、设备平台、登录风险等）
- **访问控制（Access Control）**
  - 允许访问（可加上 MFA、设备合规）
  - 阻止访问
  - 仅允许合规设备访问等

#### 示例用法：
- 在非公司网络登录 → 强制 MFA  
- 高风险登录 → 阻止访问  
- 仅允许 Intune 管理的设备访问 SharePoint

---

## 🔍 Identity Protection vs Conditional Access 区别

| 比较点 | Identity Protection | Conditional Access |
|--------|---------------------|--------------------|
| 目标 | 自动检测风险并响应 | 控制访问条件 |
| 风险识别 | 是（使用 Microsoft 威胁情报） | 支持，但依赖 Identity Protection 提供风险信号 |
| 自动处理 | 是 | 否（需管理员定义策略） |
| 策略位置 | Azure AD → Identity Protection | Azure AD → 条件访问 |
| 内建策略 | 有：3种内建策略 | 没有内建，完全自定义 |
| 示例 | 高风险用户强制密码重置 | 登录需合规设备并使用 MFA |

---

## 💼 Intune 简介

Microsoft Intune 是一个现代化的设备与应用管理平台，提供基于云的：

### ✅ 功能分类：
1. **设备管理（MDM）**
   - 管理 Windows、macOS、iOS、Android 设备
   - 下发配置：WiFi、VPN、证书、策略等
   - 控制设备合规性、加密、远程擦除等

2. **应用管理（MAM）**
   - 控制移动应用中公司数据的使用方式
   - 限制复制粘贴、加密数据、禁止截图等

3. **安全策略集成**
   - 与条件访问联动：只有 Intune 合规设备才能访问资源

4. **自助设备注册**
   - 用户可自助将设备注册入 Intune，减少 IT 人力

---

## 📝 Day 7 错题集（Markdown）

### ❌ 第 5 题  
**题目：** Azure AD 条件访问策略的“分配”部分中不包括以下哪一项？  
- A. 用户或组  
- B. 云应用  
- C. 设备合规性  
- D. 网络安全组  
**你的答案：** C  
**正确答案：** D  
**解析：** 网络安全组（NSG）是虚拟网络资源，属于网络层安全控制，不属于条件访问策略的分配目标范围。

---

### ❌ 第 10 题  
**题目：** 以下哪一项不是 Identity Protection 提供的内建策略？（选择两项）  
- A. 高风险用户强制密码重置  
- B. 高风险登录阻止访问  
- C. 异常 IP 地址封锁  
- D. 未知设备登录阻止访问  
**你的答案：** BD  
**正确答案：** CD  
**解析：** C 和 D 是可以通过条件访问策略来实现的逻辑控制，但并不是 Identity Protection 提供的三种内建策略（用户风险策略、登录风险策略、MFA 注册策略）。

---
