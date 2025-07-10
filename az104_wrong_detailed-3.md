# 📝 AZ-104 强化练习错题详解（共 3 题）

---

## ❌ 第 1 题

**题目：**  
你希望某用户仅能注册 Azure AD 应用，而不能管理其他用户，应分配哪种角色？

- A. Application Developer  
- B. Global Administrator  
- C. User Administrator  
- D. Application Administrator  

**你的答案：** `D`  
**正确答案：** `A`  
**解析：**  
应选择 A：`Application Developer`。它允许用户注册应用而不会授予不必要的权限。  
而 D（Application Administrator）权限过大，会允许用户管理所有企业应用，超出题意。

---

## ❌ 第 5 题

**题目：**  
以下关于 Diagnostic Settings 的说法，哪项是正确的？（多选）

- A. 它只能发送日志到 Log Analytics  
- B. 它可将日志发送到 Event Hub、Storage、Log Analytics  
- C. VM 必须启用 Diagnostic Extension 才能启用 Diagnostic Settings  
- D. 可设置多个目标同时接收诊断日志  

**你的答案：** `BCD`  
**正确答案：** `BD`  
**解析：**  
B 和 D 正确。C 错：虽然某些 VM 性能指标确实依赖扩展，但 Diagnostic Settings 本身适用于多种资源，并不依赖 VM Diagnostic Extension。  
A 错：除了 Log Analytics，还可以发到 Storage、Event Hub。

---

## ❌ 第 6 题

**题目：**  
你希望让开发者上传 blob，但不允许其删除或列出 blob，应如何操作？

- A. 使用 SAS token 限制权限  
- B. 分配 Storage Blob Data Contributor  
- C. 创建自定义角色仅包含上传权限  
- D. 使用 Storage Queue 控制权限  

**你的答案：** `A`  
**正确答案：** `AC`  
**解析：**  
A 是临时授权的一种方式，但最合理做法是结合 C，使用**自定义角色**明确授予上传权限（例如 Put Blob），并排除删除、列出权限。  
B 的权限过宽，包含删除；D 无关。

# 📝 AZ-104 强化练习错题详解（第 2 轮，共 4 题）

---

## ❌ 第 5 题

**题目：**  
以下哪些 Azure Policy 定义可用于资源一致性检查？（多选）

- A. 限制 VM SKU  
- B. 强制标签存在  
- C. 控制 NSG 规则  
- D. 限制资源创建时间窗口  

**你的答案：** `ABD`  
**正确答案：** `AB`  
**解析：**  
A、B 正确：可用于策略定义限制资源类型与标签要求。  
C 不常见，但可以通过策略自定义。  
**D 错误**：Azure Policy 不能限制“时间窗口”，那属于计划任务类需求，应通过部署流程控制。

---

## ❌ 第 6 题

**题目：**  
你希望开发者仅能上传 blob 文件且不可读取或删除，应使用：

- A. 存储 SAS token  
- B. 存储访问策略  
- C. 自定义角色  
- D. Storage Blob Data Contributor  

**你的答案：** `A`  
**正确答案：** `C`  
**解析：**  
虽然 A 可临时授权上传权限，但长期安全方案是使用 **自定义角色** 明确授予 Put Blob 权限，移除 List/Delete。  
B 是绑定 SAS 的基础，不是控制策略。D 权限过大。

---

## ❌ 第 8 题

**题目：**  
哪项功能允许你限制资源组中 VM 只能部署到某个子网？

- A. Azure Blueprints  
- B. NSG  
- C. Azure Policy  
- D. Role Assignment  

**你的答案：** `A`  
**正确答案：** `C`  
**解析：**  
应使用 Azure Policy 创建规则，如限制 VM 创建时使用的子网 ID。  
Blueprint 是组合部署模板的框架，但不能单独限制子网部署行为。

---

## ❌ 第 10 题

**题目：**  
你希望记录 Key Vault 中的每次读取 secret 操作，应设置：

- A. Activity Log  
- B. Diagnostic Settings  
- C. NSG Flow Logs  
- D. Application Insights  

**你的答案：** `C`  
**正确答案：** `B`  
**解析：**  
只有通过启用 **Diagnostic Settings**，才能记录对 Key Vault 中 secret 的读取行为。  
Activity Log 不含数据面操作，NSG Flow Logs 是网络级别，Insights 不适用于 KV。

