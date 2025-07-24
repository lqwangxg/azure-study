# Day 11 - Azure Bastion、VPN、ExpressRoute 与安全访问

## Azure Bastion

- 无需公网 IP 即可 RDP/SSH 登录 Azure VM
- 通过 Azure Portal 的 HTML5 浏览器安全连接
- 默认启用 TLS 加密通道
- Premium 版本支持剪贴板与文件传输

## VPN Gateway

- 提供 Site-to-Site、Point-to-Site 的加密连接
- 典型协议：IKEv2、OpenVPN
- 可配置 Active-Active、BGP 等特性
- 成本相对低，适合临时或中小型场景

## ExpressRoute

- 专线接入 Azure，不走公网，高带宽低延迟
- 支持三种 Peering 模式：Private / Microsoft / Global Reach
- 可与 VPN 共存作为备份链路
- 成本较高，适合大型企业使用

## ExpressRoute vs VPN Gateway

| 比较点        | VPN Gateway         | ExpressRoute           |
|---------------|----------------------|-------------------------|
| 通信路径       | 公网加密通道          | 物理专线，不经过公网     |
| 安全性         | 高（依赖加密）         | 更高（物理隔离）         |
| 成本           | 低                    | 高                      |
| 延迟/带宽      | 高延迟/低带宽         | 低延迟/高带宽            |
| 适用场景       | 临时/中小业务         | 长期/高吞吐需求的企业     |

## Azure Bastion Premium 特性

- 剪贴板共享 ✅
- 文件上传/下载 ✅
- 增强显示质量（特别是高分辨率） ✅
- 不自动配置 NSG ❌

## 🔌 ExpressRoute 的三种 Peering 模式区别

ExpressRoute 提供三种主要的 Peering 模式，每种适用于不同的连接需求。

---

### 1. 🏠 Private Peering（私有 Peering）

- **用途：** 连接本地数据中心与 Azure 虚拟网络（VNet）中的私有 IP 资源（如 VM、数据库等）
- **传输内容：** 仅支持 Azure 私有 IP 范围内的资源通信（10.x、172.16.x、192.168.x 等）
- **常见场景：** 企业将本地网络通过 ExpressRoute 连接至 Azure VNet，实现类似本地局域网的体验
- **端点类型：** Azure VNet 的私有 IP

✅ 支持高吞吐  
✅ 高安全性  
🚫 不可访问 Azure PaaS 服务（如 Azure SQL、Storage）

---

### 2. 🏢 Microsoft Peering（微软 Peering）

- **用途：** 访问 Microsoft 公有服务的公网终端（如 Azure Storage、Microsoft 365、Dynamics 365）
- **传输内容：** Microsoft 公网 IP 范围资源的访问，但仍通过 ExpressRoute 专线而非 Internet
- **常见场景：** 从本地访问 Microsoft 服务时需要更高的性能和可靠性
- **端点类型：** Microsoft 公网服务（IP 范围明确）

✅ 可访问 Azure PaaS 服务（Storage, SQL, WebApps 等）  
✅ 可访问 Microsoft 365 服务（需审批）  
🚫 无法访问客户自建的 Azure VNet 资源

---

### 3. 🌍 Global Reach（全局互联）

- **用途：** 允许不同区域（或国家）的 **两个本地站点**（每个都有 ExpressRoute 线路）之间互通
- **传输内容：** 两个 ExpressRoute 线路之间本地网络的互联，不经过 Azure 网络
- **常见场景：** 全球多数据中心企业希望通过 Azure Backbone 实现站点间高速互联
- **端点类型：** 企业本地网络

✅ 实现企业总部与分部之间的私有互联  
✅ 使用 Azure 全球骨干网络  
🚫 不涉及 Azure 服务访问本身

---

## 🔄 对比总结表

| 特性              | Private Peering     | Microsoft Peering     | Global Reach        |
|-------------------|---------------------|------------------------|---------------------|
| 连接对象           | Azure VNet           | Azure/Microsoft 服务   | 另一个本地数据中心   |
| 使用 IP 类型       | 私有 IP              | 公网 IP                | 本地 IP              |
| 主要用途           | VNet 内资源访问      | PaaS 服务访问          | 站点互联             |
| 是否经过 Azure     | ✅ 是                | ✅ 是                  | ❌ 不经过 Azure       |
| 典型服务访问       | VM, DB 等            | SQL, Storage, M365     | 不访问 Azure 资源     |
| 安全性/性能        | ✅ 高                | ✅ 高                  | ✅ 高                |

---

📌 **注意：**
- 使用 Microsoft Peering 访问 Microsoft 365 需额外申请并审核批准。
- 三种 Peering 可并存于一个 ExpressRoute 电路中，但配置方式不同。
