
# AZ-104 学习笔记 - Day 33

## 🛡️ 高可用性与灾难恢复（HA/DR）

---

## 🧱 高可用性架构组件

### Availability Set

- 提供主机级别的冗余。
- 特性：
  - **更新域（Update Domain）**：主机维护时避免所有 VM 同时中断。
  - **容错域（Fault Domain）**：不同电源、网络、机架物理隔离。
- 适用于部署在同一区域内的多台 VM。
- 至少部署 2 台 VM 才能发挥高可用性。

### Availability Zone

- 跨物理数据中心的高可用机制。
- 每个 Zone 是完全独立的电源、冷却、网络。
- 提供 99.99% SLA。
- 支持服务：VM、AKS、App Gateway、Load Balancer、ZRS 存储等。

### Region Pair（区域对）

- Azure 的区域是两两配对设计的（如 Japan East ↔ Japan West）。
- 特性：
  - 软件升级、维护时间错开。
  - 灾难恢复时优先恢复配对区域。
  - 支持跨区域复制服务，如 GRS（Geo-redundant Storage）、Azure SQL Geo Replication。

---

## 💾 Azure Backup 与 Site Recovery

### Azure Backup

- 支持 VM、SQL、SAP、文件夹等资源的快照和备份。
- 支持：
  - 多保留策略（Retention）
  - 加密/锁定保护
  - 还原点管理与一键恢复

### Azure Site Recovery（ASR）

- 提供 VM 的跨区域灾难恢复功能。
- 支持：
  - Azure VM → Azure VM（跨区域）
  - On-Prem 物理机 / VMware → Azure
- 三个阶段：
  1. **Test Failover**：演练测试，不影响正式业务。
  2. **Failover**：宕机发生时，实际转移至目标区域。
  3. **Failback**：灾难后业务回迁。

---

## 📈 Azure SLA 与故障容忍设计

| 方案                        | SLA           |
|-----------------------------|----------------|
| 单 VM                      | ~95%           |
| Availability Set 内部 VM    | 99.95%         |
| Availability Zone VM        | 99.99%         |
| 多区域部署（GRS、Geo SQL） | 理论接近 100% |

---

## 🌐 Azure、AWS、GCP 关于高可用性与灾难恢复（HA/DR）的对比

| 维度               | Azure                                              | AWS                                              | GCP                                              |
|--------------------|----------------------------------------------------|--------------------------------------------------|--------------------------------------------------|
| 机架/主机冗余      | Availability Set（更新域/容错域）                  | Placement Group（Spread/Partition）               | 无原生同级别，建议多实例分布                      |
| 跨数据中心高可用   | Availability Zone（AZ，独立电源/网络/冷却）        | Availability Zone（AZ，独立电源/网络/冷却）       | Zone（同一区域内多个物理区）                      |
| 跨区域容灾         | Region Pair（区域对）、多区域部署                  | 多区域部署、Route 53、S3/DB 跨区域复制            | 多区域部署、Global Load Balancer、存储多区域复制  |
| 备份服务           | Azure Backup（VM/文件/数据库等）                   | AWS Backup（EC2/EFS/RDS/FSx等）                   | Backup for GCE/Filestore/SQL、快照                |
| 灾难恢复           | Site Recovery（ASR，VM/物理机/VMware→Azure）       | Elastic Disaster Recovery（EC2/VMware→AWS）、CloudEndure | Backup/快照+多区域部署，第三方方案                |
| 自动故障转移       | 支持（SQL Geo Replication、GRS、Traffic Manager等） | 支持（RDS Multi-AZ、Route 53、Global Accelerator） | 支持（Cloud SQL HA、全球负载均衡、存储多活）      |
| SLA                | 单 VM ~95%，AZ 99.99%，多区域接近 100%             | 单实例 ~95%，多 AZ 99.99%，多区域接近 100%        | 单实例 ~99.5%，多 Zone/多区域 99.99%+             |
| 测试与演练         | 支持 Test Failover（ASR）                          | 支持演练（DRS/CloudEndure）                      | 支持演练（部分服务/第三方）                       |
| 管理与监控         | Azure Monitor、Log Analytics、Activity Log          | CloudWatch、CloudTrail、Config                   | Cloud Monitoring、Audit Logs                      |

**总结：**
- 三大云厂商均支持从主机级、数据中心级到区域级的高可用与灾难恢复能力。
- Azure 强调 Availability Set/Zone、Region Pair 及 ASR 跨平台容灾，AWS/GCP 也支持多区域多 AZ 部署和自动故障转移。
- 备份、快照、自动化恢复、SLA 保障和演练能力均完善，建议结合业务连续性
---

## ✅ 小测验结果（3 / 5）

### 1. Availability Set 的冗余功能包括：
- ❌ 你的答案：B, C, D
- ✅ 正确答案：B, D  
> C 项为操作系统兼容性，与 Set 无关

### 2. Availability Zone 的优势：
- ✅ A, B, C

### 3. Region Pair 的用途：
- ✅ A. 主备更新隔离  
- ✅ B. 灾难恢复优先区域  
- ✅ D. 异地复制服务支持

### 4. Site Recovery 的 Failover 功能：
- ❌ 你的答案：B（Test Failover）
- ✅ 正确答案：C（实际切换）

### 5. SLA 正确说明：
- ✅ B. Zone SLA 更高  
- ✅ C. GRS 支持区域恢复  
- ✅ D. 同 Zone 多 VM 可达 99.99%

---

## 📅 下一步预告 - Day 34

| 模块 | 内容 |
|------|------|
| Azure Policy 与蓝图 | 治理策略、资源合规控制 |
| 资源锁与管理组       | 防误删、安全隔离、集中管控 |
| 身份与访问管理复习   | RBAC、PIM、策略控制总结 |

