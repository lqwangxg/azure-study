# 🛡️ Azure 攻击模拟与检测知识库

## 📁 目录

- [一、攻击模拟实验：模拟横向移动](#一攻击模拟实验模拟横向移动)
- [二、日志验证清单](#二日志验证清单)
- [三、nmap 工具详解与 Defender 检测机制](#三nmap-工具详解与-defender-检测机制)

---

## 一、攻击模拟实验：模拟横向移动

### 🎯 实验目的

模拟一台受控 Azure VM 向另一台内网 VM 执行横向扫描与凭据尝试，观察 Defender for Cloud 能否检测并告警。

### 🧪 环境需求

- Azure VNet 内的两台 Linux VM（例如 Ubuntu 20.04）：
  - VM-A（攻击机，已安装 `nmap`, `hydra`, `netcat`）
  - VM-B（目标机，启用 SSH 服务）

- Defender for Cloud 启用 + Plan 2 安全计划

---

### 🛠️ 攻击模拟脚本（在 VM-A 上执行）

```bash
#!/bin/bash

echo "== 🔍 Step 1: 扫描 VM-B 是否开放端口 22 =="
nmap -Pn -p 22 <VM-B-Private-IP> -oN nmap_scan.log

echo "== 💥 Step 2: 使用 Hydra 模拟暴力破解 SSH =="
hydra -t 4 -l testuser -P /usr/share/wordlists/rockyou.txt ssh://<VM-B-Private-IP> -o hydra_results.txt

echo "== 🔗 Step 3: 模拟横向移动（尝试登录） =="
sshpass -p 'password123' ssh -o StrictHostKeyChecking=no testuser@<VM-B-Private-IP> 'uname -a'

echo "== ✅ 完成模拟横向行为 =="

```
⚠️ 注意：需自行替换 <VM-B-Private-IP> 和用户名/密码。


## 二、日志验证清单

| 类别     | 验证项目             | Azure 来源                        | 查询或位置                       |
| ------ | ---------------- | ------------------------------- | --------------------------- |
| NSG 日志 | 端口 22 被连接        | NSG Flow Logs                   | Network Watcher > Flow Logs |
| 安全告警   | SSH 暴力破解尝试       | Defender for Cloud              | Security alerts             |
| 安全告警   | 可疑横向移动           | Defender for Cloud              | Security alerts             |
| 行为分析   | 同一 IP 多端口访问      | Microsoft Sentinel (KQL)        | `AzureNetworkAnalytics_CL`  |
| 虚机日志   | 登录记录             | Linux 审计日志（/var/log/auth.log）   | 可使用 Azure Monitor 收集        |
| EDR 行为 | hydra / nmap 被调用 | Microsoft Defender for Endpoint | Timeline / Advanced Hunting |

## 三、nmap 工具详解与 Defender 检测机制
### 🔍 什么是 nmap？
nmap（Network Mapper）是一个开源的网络扫描和安全审核工具。它被广泛用于：
- 探测在线主机（Host Discovery）
- 识别开放端口（Port Scanning）
- 检测操作系统类型（OS Detection）
- 枚举服务与版本（Service Version Detection）
- 安全漏洞测试（通过 NSE 脚本）

### ⚙️ 工作原理
通过构造并发送各种网络数据包（TCP、UDP、ICMP 等）到目标系统，然后根据返回包判断目标主机是否存在、端口是否开放、服务类型等。

### 常见扫描方式
| 类型                 | 描述            |
| ------------------ | ------------- |
| Ping 扫描（ICMP Echo） | 判断主机是否存活      |
| TCP Connect 扫描     | 使用完整 TCP 三次握手 |
| SYN 扫描（Stealth）    | 半开扫描，更隐蔽      |
| UDP 扫描             | 探测 UDP 服务     |
| 服务版本探测（-sV）        | 获取服务 banner   |
| 操作系统识别（-O）         | 基于 TCP/IP 栈特征 |
| NSE 脚本（--script）   | 执行安全脚本，如漏洞检测  |

### 🧪 常用命令参数
| 命令                           | 功能说明                  |
| ---------------------------- | --------------------- |
| `nmap <ip>`                  | 默认 TCP 端口（0-1000）扫描   |
| `nmap -Pn <ip>`              | 不进行 ping 检测           |
| `nmap -sS -p 22,80,443 <ip>` | SYN 扫描特定端口            |
| `nmap -sV <ip>`              | 探测服务版本                |
| `nmap -O <ip>`               | 探测操作系统                |
| `nmap -A <ip>`               | 高级信息收集（包含 -sV -O -sC） |
| `nmap --script=vuln <ip>`    | 检测已知漏洞                |
| `nmap -T4 <ip>`              | 加快扫描速度                |

### 🔍 Defender for Cloud 如何检测 nmap 扫描？
#### 📊 1. NSG Flow Logs + 威胁智能
- Azure 收集五元组流量（源/目标 IP、端口、协议）
- 分析短时间大量端口连接行为（特征模式）
- 告警：如 "Port scanning from external IP"

#### 🧠 2. Defender for Endpoint（如部署）
- 监控系统调用（如 nmap, hydra 命令执行）
- 捕捉进程行为（恶意脚本或工具）
- 报告到 Defender 中的 Timeline / Incident

#### 📉 3. Microsoft Sentinel 检测方式（可选）
- 查询异常端口访问

```kql
AzureNetworkAnalytics_CL
| where DstPortNumber_s in ("22", "3389", "445")
| summarize count() by SrcIpAddr_s, DstIpAddr_s, DstPortNumber_s
```

### ✅ 防护建议
| 场景      | 建议                                   |
| ------- | ------------------------------------ |
| 暴露公网 IP | NSG 限制端口、启用 Defender for Cloud       |
| 内网扫描    | 安装 MDE 代理，使用 Sentinel 警报             |
| 服务探测    | 使用 Bastion 或 Azure Firewall 隐藏目标主机   |
| 非授权登录   | 启用 MFA，限制 SSH 用户，日志上传到 Log Analytics |
