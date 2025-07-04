
# AZ-104 学习笔记 - Day 22

## 💽 今日主题：Azure Disk 管理与虚拟机备份架构

---

## 🧱 Azure Disk 类型与用途

| 磁盘类型 | 用途 | 特性 |
|----------|------|------|
| OS Disk  | 操作系统盘 | 支持快照、加密 |
| Data Disk| 数据盘       | 可挂载多个，支持快照 |
| Temp Disk| 临时盘       | 非持久性，仅供缓存/分页，重启会清除（如 D: 盘）|

---

## ⚙️ Azure Disk 性能层级

| 类型          | 适用场景             | 最大 IOPS / 吞吐量 |
|---------------|----------------------|---------------------|
| Standard HDD  | 测试、冷数据         | 500 IOPS            |
| Standard SSD  | 业务中等负载         | 6,000 IOPS          |
| Premium SSD   | 高性能需求，数据库等 | 高达 20,000+ IOPS   |
| Ultra Disk     | 极端低延迟场景       | 高达 160,000+ IOPS  |

---

## 📸 Azure Disk 快照机制

- 快照是 **磁盘级别的副本**，可用于恢复或克隆新磁盘。
- 增量快照节省存储成本。
- 快照存储在 **LRS 或 ZRS 存储账户**中。
- 可通过 Portal、CLI、PowerShell 管理。

```bash
az snapshot create --resource-group myRG --source myDisk --name mySnapshot
```

---

## 🛡️ Azure Backup for Virtual Machines

Azure 提供完整 VM 的备份服务，支持：

- ✅ 自动计划备份（每日/每周）
- ✅ 整机还原、磁盘还原、文件还原
- ✅ 快照 + 增量备份
- ✅ 软删除保护与 Vault 锁定

> 适用于生产环境 VM 的长期保护和合规性管理。

---

## 🔐 Recovery Services Vault 作用

| 功能 | 描述 |
|------|------|
| 备份元数据存储 | 包含备份策略、恢复点信息 |
| 管理备份策略 | 调度与保留规则 |
| 支持资源恢复 | 一键恢复 VM、磁盘或文件 |
| 防止误删       | Soft Delete / Immutable Vault 支持防篡改保护 |

---

## 💰 成本优化建议

- 使用 **增量快照** 降低磁盘快照成本
- 清理历史恢复点，避免 Vault 占用膨胀
- 使用生命周期策略统一控制保留周期
- 数据盘按需分层（Standard SSD / Premium）

---

## 🌐 Azure、AWS、GCP 文件备份与快照机制对比

| 维度           | Azure                                    | AWS                                         | GCP                                         |
|----------------|------------------------------------------|---------------------------------------------|---------------------------------------------|
| 文件共享快照   | Azure Files 支持共享级快照，增量计费     | EFS 支持文件系统快照，支持自动/手动         | Filestore 支持文件共享快照（部分版本）      |
| 快照粒度       | 共享级（支持文件级还原）                 | 文件系统级（支持文件/目录级还原）           | 文件共享级（支持文件/目录级还原）           |
| 快照触发方式   | 手动、API、CLI、Portal                   | 手动、API、CLI、自动计划                    | 手动、API、CLI                              |
| 快照保留策略   | 手动删除或结合生命周期策略                | 支持生命周期管理、自动快照策略              | 支持快照保留、自动清理（部分版本）           |
| 备份服务       | Azure Backup for Azure Files，集成 Recovery Vault，支持自动调度、文件/文件夹级还原 | AWS Backup 支持 EFS/FSx，集中管理，自动调度、生命周期管理 | Backup for Filestore，自动/手动备份，集中管理 |
| 备份粒度       | 文件/文件夹级                            | 文件系统级、目录级                          | 文件共享级、目录级                          |
| 备份保留       | 最长 180 天（可策略设定）                 | 灵活设定，支持长期归档                      | 灵活设定，支持长期归档                      |
| 恢复方式       | Portal、PowerShell、Storage Explorer      | 控制台、CLI、API                            | 控制台、gcloud、API                         |
| 成本           | 快照按增量存储计费，备份按存储+管理计费   | 快照/备份按存储和操作计费                   | 快照/备份按存储和操作计费                   |

**总结：**
- 三大云厂商均支持文件共享快照和集中备份，支持自动调度、生命周期管理和细粒度恢复。
- Azure Files 快照和 Azure Backup 集成 Recovery Vault，AWS Backup 可统一管理 EFS/FSx 备份，GCP Filestore 支持快照和集中备份。
- 建议结合数据恢复需求、自动化程度和成本优化选择合适的快照与备份方
---

## ✅ 小测验回顾（5 / 5）

### 1. 临时磁盘作用？
- ✅ B. 加速页面文件或缓存使用

### 2. 支持托管磁盘快照的类型？
- ✅ A. OS Disk
- ✅ B. Data Disk

### 3. VM 启用 Azure Backup 的前提？
- ✅ A. Recovery Services Vault 创建完成
- ✅ B. 磁盘为托管类型
- ✅ C. VM 与 Vault 在同一区域

### 4. Azure Backup for VM 的优势？
- ✅ A. 支持计划备份
- ✅ B. 一键还原整个 VM
- ✅ D. 支持磁盘级别还原

### 5. Recovery Services Vault 不具备的功能？
- ✅ B. 存储虚拟机镜像

---

## 📅 下一步预告 - Day 23

| 模块 | 内容 |
|------|------|
| VM 自动化管理 | 计划启动、关机、扩容策略 |
| Azure Automation | Runbook、Update 管理 |
| 故障恢复（DR） | 跨区域备份 + Site Recovery |

