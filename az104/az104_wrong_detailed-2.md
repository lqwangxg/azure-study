# AZ-104 模拟考试错题详解（共 13 题）

## ❌ 第 4 题
**题目：** 哪个角色可以创建 Azure AD 应用注册？（多选）
- A. Application Developer
- B. Global Administrator
- C. Contributor
- D. Application Administrator

**你的答案：** `ABCD`
**正确答案：** `ABD`
**解析：** Contributor（C）无权限在 Azure AD 中注册应用。正确答案是 ABD。

## ❌ 第 5 题
**题目：** 你希望仅允许用户更改自己的密码，应分配下列哪个角色？
- A. User Access Administrator
- B. Helpdesk Administrator
- C. Password Reset Administrator
- D. Security Reader

**你的答案：** `B`
**正确答案：** `C`
**解析：** Password Reset Administrator 专门用于重置和修改用户自身凭据。Helpdesk 管理员权限更广，不适用于仅允许更改自己密码的最小权限模型。

## ❌ 第 9 题
**题目：** 你希望让 Azure VM 使用其系统分配的托管身份访问 Key Vault，需要做哪些操作？（多选）
- A. 启用托管身份
- B. 为托管身份分配 Key Vault Reader
- C. 配置 Key Vault 访问策略或 RBAC
- D. 修改 VM SKU 为 Premium

**你的答案：** `ABC`
**正确答案：** `AC`
**解析：** Key Vault Reader 不包含对机密读取的权限，应使用 Key Vault Secrets User。A 和 C 是必须的。

## ❌ 第 14 题
**题目：** 下列哪种 Azure 存储类型适合频繁写入大量小文件，如 IoT 传感数据？
- A. Append Blob
- B. Page Blob
- C. Queue Storage
- D. File Share

**你的答案：** `C`
**正确答案：** `A`
**解析：** Append Blob 是为追加写设计，适合日志、IoT 等场景。Queue Storage 用于消息队列。

## ❌ 第 17 题
**题目：** 下列哪个服务不依赖 NSG 即可工作？
- A. VM
- B. Application Gateway
- C. Azure Firewall
- D. Azure Files

**你的答案：** `C`
**正确答案：** `D`
**解析：** Azure Files 是 PaaS 服务，不受 NSG 控制，**NSG 主要作用于 IaaS 层虚拟机网络**。

## ❌ 第 23 题
**题目：** 若你要将虚拟机日志发送到 Log Analytics，应配置：
- A. Azure Activity Log
- B. Diagnostic Settings
- C. Network Watcher
- D. Azure Monitor Metrics

**你的答案：** `C`
**正确答案：** `B`
**解析：** 应使用 Diagnostic Settings 发送数据到 Log Analytics。Network Watcher 用于网络监控，Activity Log 主要记录操作事件。

## ❌ 第 24 题
**题目：** 你希望使用 Backup 保护一台 VM，同时启用“软删除”，其作用是？
- A. 提高备份速度
- B. 防止 VM 被意外关闭
- C. 防止备份点被意外删除
- D. 保证每小时自动备份

**你的答案：** `B`
**正确答案：** `C`
**解析：** 软删除确保备份数据即使被删除也可以在一定时间内恢复，防止误删。

## ❌ 第 40 题
**题目：** 你希望某开发人员访问某个 Blob Container，但禁止其删除任何 blob，应分配：
- A. Storage Blob Data Contributor
- B. Storage Blob Data Reader
- C. Reader
- D. 自定义角色：仅允许读取和上传

**你的答案：** `B`
**正确答案：** `D`
**解析：** Storage Blob Data Reader 只能查看内容，不能上传或修改；最合理做法是自定义角色，仅授上传/读权限。

## ❌ 第 48 题
**题目：** Azure CLI 默认使用哪种语言格式返回 JSON 数据？
- A. jq
- B. PowerShell
- C. KQL
- D. JSON

**你的答案：** `B`
**正确答案：** `D`
**解析：** Azure CLI 返回数据本身就是 JSON，`jq` 是处理工具，`D` 才是格式本身。

## ❌ 第 51 题
**题目：** 你希望将多个订阅内的资源视图统一，并集中管理权限，应使用：
- A. Management Groups
- B. Azure Lighthouse
- C. Resource Groups
- D. Azure Blueprints

**你的答案：** `B`
**正确答案：** `A`
**解析：** Management Groups 用于组织结构、策略控制；Lighthouse 用于跨租户委托管理。

## ❌ 第 52 题
**题目：** 题干略

**你的答案：** `A`
**正确答案：** `B`
**解析：** 解析略

## ❌ 第 58 题
**题目：** 你希望在资源部署失败时立即触发警报，应使用：
- A. Azure Monitor Alert
- B. Resource Lock
- C. Azure Advisor
- D. Activity Log Alert

**你的答案：** `D`
**正确答案：** `A`
**解析：** 部署失败事件写入 Activity Log，需用 Activity Log Alert 触发警报。

## ❌ 第 59 题
**题目：** 你想追踪一个存储账户中所有 blob 的访问日志，应启用：
- A. Azure Backup
- B. Blob Lifecycle Policy
- C. Diagnostic Settings（Blob logs）
- D. NSG Flow Logs

**你的答案：** `D`
**正确答案：** `C`
**解析：** 应启用 Diagnostic Settings，并将 blob 操作记录写入 Log Analytics 或 Storage。
