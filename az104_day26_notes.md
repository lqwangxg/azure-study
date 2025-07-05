# AZ-104 学习笔记 - Day 26

## 💾 今日主题：Azure 备份、恢复与存储监控

---

## ☂️ Azure Backup 与 Recovery Services Vault

- Recovery Services Vault 是 Azure 备份和恢复管理的核心。
- 支持资源：
  - Azure 虚拟机（整盘或系统状态备份）
  - SQL Server 和 SAP HANA 数据库备份
  - Azure Files 文件夹级别备份
  - Blob 存储（预览中的 Blob 备份功能）
- 特色：
  - 增量备份减少存储和带宽占用
  - 灵活的保留策略（从几天到永久）
  - 支持跨区域恢复（GRS）

---

## 🌀 快照（Snapshot）与软删除（Soft Delete）

- **快照**：
  - 只读的时间点副本，主要用于 VM 磁盘和 Blob 存储
  - 支持数据回滚和恢复
- **软删除**：
  - 针对 Blob 存储的删除保护机制
  - 删除后数据可在设定的保留期（默认 7-30 天）内恢复
- **版本控制**：
  - Blob 上传和修改时会自动保存历史版本，支持回滚

---

## 📈 存储监控与日志审计

- **Azure Monitor** 用于监控存储账户的性能指标（吞吐量、IOPS、延迟等）
- **诊断设置**：启用存储账户的访问日志、操作日志输出到 Log Analytics、存储账户或事件中心
- **访问日志**：跟踪谁访问了存储，访问的时间和操作类型
- **威胁侦测**：识别异常行为，如暴力破解尝试和匿名访问

---

## 🚀 存储扩展性与性能限制

| 限制项目          | 标准存储账户（General Purpose v2） |
|-------------------|------------------------------------|
| 最大容量          | 5 PB                              |
| 请求速率（IOPS） | 单分区约 20,000                   |
| 吞吐量            | 最高约 50 Gbps（读取）             |
| Blob 单对象大小   | 最大 4.75 TB（200 GB 块大小限制）  |

- **Premium 存储** 适合对延迟和 IOPS 有高要求的应用（如数据库）
---

## 🌐 Azure、AWS、GCP 存储服务与访问控制策略对比

| 维度           | Azure Storage                                 | AWS Storage                                 | GCP Storage                                 |
|----------------|----------------------------------------------|---------------------------------------------|---------------------------------------------|
| 对象存储       | Blob Storage（Block/Append/Page Blob）        | S3（Standard/IA/Glacier）                   | Cloud Storage（Standard/Nearline/Coldline/Archive） |
| 文件存储       | Azure Files（SMB/NFS）                        | EFS（NFS）、FSx（SMB/NFS）                  | Filestore（NFS）                            |
| 块存储         | Azure Disk（托管磁盘）                        | EBS（Elastic Block Store）                  | Persistent Disk                             |
| 访问控制模型   | RBAC（基于角色）、存储账户密钥、SAS、Azure AD | IAM Policy、Bucket Policy、Access Key、临时凭证 | IAM Policy、签名URL、服务账号               |
| 细粒度授权     | 支持到容器/Blob 级别（RBAC/SAS）              | 支持到桶/对象级别（IAM/Policy）             | 支持到桶/对象级别（IAM/Policy）             |
| 身份集成       | 支持 Azure AD、AD DS、RBAC                    | 支持 IAM、AD（FSx for Windows）             | 支持 IAM、AD（Filestore Enterprise）         |
| 临时访问       | SAS（共享访问签名，带权限/时间限制）          | 预签名 URL、临时凭证（STS）                 | 签名 URL、临时凭证                          |
| 访问日志与审计 | 支持存储分析、Activity Log、Monitor            | S3 Access Log、CloudTrail、CloudWatch       | Audit Logs、存储访问日志                    |
| 加密           | 默认 SSE，支持 CMK（Key Vault）、传输加密      | 默认 SSE，支持 KMS/BYOK，传输加密           | 默认 SSE，支持 CMEK（KMS），传输加密         |

**总结：**
- 三大云厂商均支持对象、文件、块存储，访问控制均可细粒度到桶/容器/

---

## ✅ 小测验回顾

1. Recovery Vault 可备份哪些资源？
   - Azure VM、SQL Server、Azure Files（不包括虚拟网络）
2. Snapshot 与 Soft Delete 的区别：
   - Snapshot 是只读副本，支持磁盘回滚
   - Soft Delete 自动保护 Blob 数据，非手动触发
3. 启用访问日志需在诊断设置中开启，不是 Application Insights
4. 使用 `StorageBlobLogs` 结合 KQL 可查看访问记录
5. 存储性能：
   - 标准账户最大 5 PB 容量
   - Blob 最大 4.75 TB
   - Premium 存储提供更高 IOPS

---

## 📅 下一步预告 - Day 27

| 模块 | 内容 |
|------|------|
| Azure 网络安全组（NSG）深入 | 规则优先级与流量分析 |
| 应用安全组（ASG） | 资源分组与细粒度访问控制 |
| 网络流量监控 | 流日志与监控工具 |

---

