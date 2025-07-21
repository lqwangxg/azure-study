# 📘 AZ-500 Day 3：Azure AD Privileged Identity Management（PIM）

## 📌 学习重点知识点

### ✅ 什么是 PIM？
Azure AD Privileged Identity Management (PIM) 是一项用于管理 Azure AD 和 Azure 资源中特权角色访问权限的服务，强调最小权限与 Just-In-Time 控制。

### ✅ PIM 的核心功能
- 🕒 **Just-In-Time (JIT) 权限提升**：不再持续拥有权限，而是在需要时激活。
- 🔐 **多重身份验证 (MFA)**：激活角色前可要求用户执行 MFA。
- 📝 **激活理由说明**：可配置要求用户说明为何激活角色。
- ✅ **审批流程支持**：可配置角色激活需审批。
- 📆 **时间范围设置**：可设定权限激活持续时间。
- 🧹 **自动移除未使用角色分配**：减少长期特权。
- 📊 **完整的审计日志和警报通知**。

### ✅ PIM 支持的角色和资源类型
- Azure AD 管理员角色（如：Global Administrator）
- Azure 资源 RBAC 角色（如：VM Contributor、Storage Blob Data Reader）
- ❌ 不支持 Microsoft 365（如 Exchange、SharePoint）角色管理

---

## 📝 Day 3 模拟练习题（含题干）

### 1. 在 Azure AD 中，Privileged Identity Management 的主要目标是什么？
A. 增加所有用户的访问权限  
B. 实现 Just-In-Time 的权限提升  
C. 自动为所有用户分配管理员角色  
D. 限制对多重身份验证的使用  

✅ 正确答案：B  

---

### 2. PIM 提供哪些关键功能？（多选）
A. 永久分配所有角色  
B. Just-In-Time 访问控制  
C. 激活前的审批机制  
D. 自动移除未使用的角色  

✅ 正确答案：B, C, D  
❌ 你的答案：B, C  
🔍 解析：PIM 不推荐永久分配特权，选项 A 错。

---

### 3. 下列哪项可以通过 PIM 实现？
A. 自动检测并隔离恶意用户  
B. 管理资源的成本分配  
C. 临时授予 VM Contributor 权限  
D. 为 Key Vault 设置防火墙策略  

✅ 正确答案：C  

---

### 4. 你希望用户在激活 Global Administrator 前提供说明和 MFA，应该怎么做？
A. 使用 Azure Policy 限制权限  
B. 使用 Intune 配置角色激活  
C. 在 PIM 中启用激活设置：MFA 与理由  
D. 为用户授予永久权限  

✅ 正确答案：C  

---

### 5. 以下哪个角色不能由 PIM 管理？
A. Billing Administrator  
B. Exchange Online 管理员  
C. User Administrator  
D. Application Administrator  

✅ 正确答案：B  

---

### 6. 哪些是角色分配类型？（多选）
A. Eligible  
B. Permanent  
C. Active  
D. Conditional  

✅ 正确答案：A, C  
❌ 你的答案：A, B  
🔍 解析：PIM 不使用“Permanent”术语，只有 Eligible（合格）和 Active（已激活）两种类型。

---

### 7. PIM 可以管理以下哪些角色？（多选）
A. Azure AD 管理员角色  
B. Azure 资源上的 RBAC 角色  
C. Microsoft 365 Exchange 管理员角色  
D. Resource Group Contributor  

✅ 正确答案：A, B, D  
❌ 你的答案：A, D  

---

### 8. 在 PIM 中，激活角色时可以要求哪些额外的安全措施？（多选）
A. 多重身份验证 (MFA)  
B. IP 地址白名单  
C. 填写激活理由  
D. 安装客户端代理  

✅ 正确答案：A, C  

---

### 9. 用户在 PIM 中被标记为“Eligible”，下列哪项正确？
A. 用户自动拥有角色权限  
B. 用户永远无法激活角色  
C. 用户需要手动激活角色权限  
D. 用户只能请求审批人分配权限  

✅ 正确答案：C  
❌ 你的答案：B  

---

### 10. 哪些操作可以触发 PIM 的通知？（多选）
A. 用户激活角色  
B. 分配永久角色  
C. 管理 Intune 策略  
D. 用户请求激活角色并等待审批  

✅ 正确答案：A, B, D  
❌ 你的答案：A, D  

---

## ❗ 错题总结（共 4 题）

| 题号 | 你的答案 | 正确答案 | 错因简析 |
|------|-----------|------------|-----------|
| 2    | B, C       | B, C, D     | 忽略了自动移除长期未使用角色是 PIM 亮点功能 |
| 6    | A, B       | A, C        | 没有“Permanent”这个分配类型 |
| 7    | A, D       | A, B, D     | 忽略了 PIM 支持 Azure 资源级别角色（如 VM Contributor） |
| 9    | B          | C           | Eligible 用户需要手动激活角色权限才能生效 |

---

## ✅ 下一步建议

- 📖 复习错题涉及的核心点（Eligible、角色类型、支持的资源）
- 🛠️ 可在 Azure 门户中动手操作一次角色激活流程，加深理解
- 👉 明天将进入 **Day 4：Azure AD Identity Protection（身份保护机制）**

