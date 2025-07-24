# 📘 AZ-500 学习计划 - Day 9

## ✅ 今日知识点：Microsoft Defender for Cloud & 工作负载保护

| 主题 | 关键点 |
|------|--------|
| **Microsoft Defender for Cloud** | 是 Azure 的 CSPM（云安全态势管理）平台，提供 **安全评分、安全建议、威胁防护、多云支持**（包括 AWS、GCP）。 |
| **Auto Provisioning（自动配置）** | 可自动为新建 VM 安装监控代理（如 Log Analytics Agent 或 Defender Extension）。 |
| **Defender for Servers** | 启用后会自动部署 MMA 或 Azure Monitor Agent，集成 Defender for Endpoint 提供高级端点防护。 |
| **各 Defender 模块功能示例** | - **Defender for Key Vault**：检测对敏感机密的读取或写入行为。<br>- **Defender for Storage**：监控恶意文件活动和访问异常。<br>- **Defender for SQL**：识别 SQL 注入与恶意查询行为。 |
| **Azure Policy 与 Defender 集成** | 可利用 Azure Policy 强制资源部署时启用 Defender、安全代理或合规控制。 |
| **Secure Score** | 安全评分衡量资源环境安全状态，受 NSG 设置、更新情况、加密策略等因素影响。 |
| **Regulatory Compliance（法规合规性）** | Defender 可对标多个行业标准（如 ISO27001、NIST SP800-53），检测和报告不合规项。 |
| **多云安全监控** | 通过连接器集成 AWS 和 GCP 的工作负载以统一进行威胁检测与合规分析。 |

---

## ❌ Day 9 错题集

### ❌ 第 7 题

**你的答案：ABD**  
**正确答案：BCD**

**题目内容：**  
Azure Defender（即 Microsoft Defender for Cloud）支持下列哪些资源类型？请选择三项。

A. Azure Blob Storage  
B. Azure Kubernetes Service  
C. Azure Firewall  
D. Azure SQL Database  
E. Azure DNS

**解析：**  
Microsoft Defender 支持以下资源类型的计划：  
- **B: Azure Kubernetes Service (AKS)** — 可启用 Defender for Containers  
- **C: Azure Firewall** — 可进行日志分析与威胁检测  
- **D: Azure SQL Database** — 可识别 SQL 注入、异常行为等攻击

Azure DNS 并不支持 Defender，Blob Storage 受 Defender for Storage 管控，题干未明确提及类型，因此排除 A 和 E。

---

