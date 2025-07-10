# AZ-104 考前错题回顾清单

## ❗ 权限控制与身份认证

### ❌ Function App 最佳权限控制  
- 错选：A. 生成 SAS 链接  
- 正解：**B. 使用 Managed Identity + 角色分配（Storage Blob Data Reader）**  
- ✅ 原因：最小凭据原则，避免硬编码  

### ❌ Initiative 可用于权限控制？  
- 错选：C. 可控制访问权限  
- 正解：**❌ Initiative 仅用于聚合 Policy，无法控制权限**  

---

## ❗ 存储与加密机制

### ❌ 存储默认加密机制  
- 错选：D. 不加密  
- 正解：**C. Microsoft-managed key（平台托管）**  
- ✅ Azure 所有资源默认加密，用户可改为 CMK  

### ❌ VM 是 PaaS？  
- 错选：将 VM 当作 PaaS  
- 正解：**VM 是 IaaS**，而 App Service / SQL / Logic App 是 PaaS

---

## ❗ 网络与访问控制

### ❌ NSG 支持 FQDN？  
- 错选：A. NSG 可配置 FQDN  
- 正解：**❌ NSG 不支持 FQDN，仅 IP/Port 级别**  
- ✅ 若需域名过滤，请使用 Azure Firewall

---

## ❗ 告警与监控

### ❌ Function App 报错触发通知  
- 错选：仅 Metric 告警  
- 正解：**Log 告警 + Action Group 可根据日志中错误内容触发邮件/自动化**

---

# AZ-104 模拟考试-1-错题回顾（共 9 题）

## ❌ 2. 虚拟机操作权限控制
**题目：**  
你希望允许某个用户重启 Azure VM，但不允许其删除 VM。你应分配哪种内建角色？  
**你的答案：** A. Contributor  
**正确答案：** B. Virtual Machine Contributor  
**解析：** Contributor 拥有广泛权限，包括删除。Virtual Machine Contributor 专门用于管理 VM，不含删除权限，更符合最小权限原则。

---

## ❌ 3. 限制存储账户访问来源
**题目：**  
你希望防止任何人从公共网络访问某个存储账户，但允许专用网络（VNet）访问。应启用：  
**你的答案：** C. NSG Inbound Rule  
**正确答案：** B. Private Endpoint  
**解析：** NSG 无法限制存储服务的公私网络访问。正确做法是启用 Private Endpoint，从而实现完全关闭公网访问，仅限专用网络访问。

---

## ❌ 4. 托管身份支持的服务
**题目：**  
以下哪些服务支持使用托管身份（Managed Identity）进行身份验证？（多选）  
**你的答案：** BCD  
**正确答案：** ABCD  
**解析：** Azure VM、Logic App、SQL Database、AKS 都支持托管身份。选项 A（VM）漏选。

---

## ❌ 7. Azure Backup 恢复失败的排查
**题目：**  
你在使用 Azure Backup 保护一台 Windows VM，但恢复失败。你最可能要检查的内容是？  
**你的答案：** A. VM 是否在运行  
**正确答案：** C. Azure Backup Agent 是否安装  
**解析：** 使用 Azure Backup 恢复时需依赖代理。未安装或配置异常将导致恢复失败。

---

## ❌ 11. 资源运行状况分析服务
**题目：**  
你希望分析 Azure 上所有资源的运行状况，应使用哪项服务？（多选）  
**你的答案：** A  
**正确答案：** A, B, C  
**解析：** Azure Monitor（A）+ Log Analytics（B）收集与分析日志数据，Service Health（C）用于识别服务层面问题。

---

## ❌ 12. 限制 SSH 登录来源
**题目：**  
如何限制 VM 只能从特定 IP 段登录 SSH？  
**你的答案：** B. 使用 Azure Firewall  
**正确答案：** A. 配置 NSG 入站规则  
**解析：** 限定访问来源首选 NSG，Firewall 适用于更复杂的流量控制。

---

## ❌ 15. 日志读取权限控制
**题目：**  
你想限制某用户只能读取诊断日志，不允许做其他操作，应该分配的角色是？  
**你的答案：** D. Storage Blob Data Reader  
**正确答案：** B. Monitoring Reader  
**解析：** Monitoring Reader 可读取监控日志，Storage Blob Data Reader 用于读取 Blob 内容，不适合诊断日志访问控制。

---

## ❌ 17. Function App 内网访问控制
**题目：**  
你希望让某个 Function App 仅由 VNet 中的资源访问，应使用：  
**你的答案：** A. IP 限制  
**正确答案：** C. Private Endpoint  
**解析：** IP 限制不够安全。Private Endpoint 可将 Function App 完全绑定至 VNet，禁止公网访问。

---

# AZ-104 强化练习错题集（共 4 题）

---

## ❌ 1.（第2组第1题）限制用户仅重置 VM 密码

**题目：**  
你希望某个用户只能为虚拟机重置密码，但不能更改其他配置，应使用哪个角色？

**你的答案：** B. Virtual Machine Contributor  
**正确答案：** A. Helpdesk Administrator  

**解析：**  
Helpdesk Administrator 是最小权限模型下专门用于重置用户凭据的角色。  
Virtual Machine Contributor 权限范围更大，可能允许修改配置，不符合要求。

---

## ❌ 2.（第1组第4题）Azure VM 是否需要安装 Backup Agent

**题目：**  
启用 Azure Backup 保护 VM 时，下列哪项不需要你手动配置？

**你的答案：** A. Recovery Services Vault  
**正确答案：** B. Azure Backup Agent  

**解析：**  
Azure VM 的备份通过平台集成，不需要用户手动安装 Azure Backup Agent。该代理只用于本地服务器的备份。

---

## ❌ 3.（第1组第11题）Private Endpoint 的正确流量监控方式

**题目：**  
若你希望监控某 VNet 的出入流量，推荐使用的服务是？（多选）

**你的答案：** A, C, D  
**正确答案：** A, B, C  

**解析：**  
Traffic Manager 是 DNS 层负载均衡工具，不监控流量。  
推荐使用 NSG Flow Logs、Azure Monitor Metrics、Network Watcher 来监控 VNet 流量。

---

## ❌ 4.（第1组第3题）Azure Backup 的恢复失败原因排查

**题目：**  
你在使用 Azure Backup 保护一台 Windows VM，但恢复失败。你最可能要检查的内容是？

**你的答案：** A. VM 是否在运行  
**正确答案：** C. Azure Backup Agent 是否安装  

**解析：**  
如果是本地物理或异地 VM，需要安装 Azure Backup Agent。对于 Azure 内部 VM 则不会是运行状态问题，而是依赖备份代理（或缺失诊断配置）导致恢复失败。

---
