# AZ-104 学习笔记 - Day 50（冲刺测试）

## 🧪 精炼版模拟测试（20 题）

- 模拟题量：20 题  
- 答题时间：约 30 分钟  
- 通过标准：≥70%（14/20）  
- 得分：✅ 18 / 20（90%） → 已达通过水准

---

## ✅ 答题与解析

| 题号 | 你的答案 | 正确答案 | 要点说明 | 得分 |
|------|-----------|-----------|-----------|------|
| 1    | B         | B         | 使用 Policy 限定部署区域 | ✅ |
| 2    | BCD       | BCD       | VM ❌ 不支持 PE | ✅ |
| 3    | ABC       | ABC       | MI 可用于资源访问，但不能登录 Portal | ✅ |
| 4    | C         | C         | 指标类监控选 Metrics | ✅ |
| 5    | C         | C         | Reader 可读不可修改 | ✅ |
| 6    | BCD       | BCD       | NSG ❌ 不支持 FQDN | ✅ |
| 7    | A         | A         | 使用 Policy 强制规则 | ✅ |
| 8    | B         | B         | VM 备份基于 Recovery Services Vault | ✅ |
| 9    | A         | A         | 删除保护使用 Lock | ✅ |
| 10   | A         | ❌ B       | 推荐使用 MI + 权限控制 | ❌ |
| 11   | ABD       | ABD       | App Insights 不支持 Firewall | ✅ |
| 12   | B         | B         | 使用 Resource Graph + KQL | ✅ |
| 13   | ABD       | ABD       | Initiative 仅用于策略聚合 | ✅ |
| 14   | BCD       | ❌ ABD     | VM 属于 IaaS | ❌ |
| 15   | A         | A         | 操作日志使用 Activity Log | ✅ |
| 16   | C         | C         | 权限继承层级：管理组 > 订阅 > 资源组 > 资源 | ✅ |
| 17   | C         | C         | Azure Monitor 使用 KQL 查询 | ✅ |
| 18   | B         | B         | Logic App 可定时启用 VM | ✅ |
| 19   | D         | ❌ C       | 默认加密是平台托管密钥 | ❌ |
| 20   | C         | C         | 日志告警 + Action Group 实现通知 | ✅ |

---

## 📊 成绩统计

- 正确题数：18 / 20  
- 正确率：90%  
- 合格线：70%  
- 是否合格：✅ 是，推荐参加 AZ-104 正式考试

---

## ❗ 错题分析

### 第 10 题：Function App 最佳权限控制  
- 你选了 A（SAS 链接）  
- 正确：B（使用 Managed Identity）  
- 建议：为 Function App 分配 System-assigned MI 并给予存储访问权限  

### 第 14 题：PaaS 服务识别  
- 你选了 BCD，错将 VM 当作 PaaS  
- 正确答案：ABD（App Service、SQL Database、Logic App）  
- VM 属于 IaaS，不包含在 PaaS 范畴内  

### 第 19 题：默认加密机制  
- 你选了 D（不加密）  
- 正确：C（平台托管密钥）  
- Azure 所有资源默认加密，使用 Microsoft-managed keys  

---

## ✅ 总结建议

- 备考状态：已达考试水准
- 推荐：进入考前冲刺 + 错题记忆阶段
- 工具推荐：Azure 官方 Docs + Microsoft Learn 路径 + 重点截图整理

---

## 🔜 下一步：考试前策略

| 项目           | 动作建议                        |
|----------------|---------------------------------|
| 模拟冲刺       | 每日快速做 10–20 题回顾         |
| 思维地图        | 构建 6 大模块知识点链接图       |
| 知识巩固       | 错题反复看，相关资源实操1次     |
| 考试预约       | 进入 Pearson VUE 平台，预约考试 |
