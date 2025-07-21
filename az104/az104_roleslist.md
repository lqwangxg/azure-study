Azure 提供了 100 多种内置角色（Built-in Role），常用的部分如下：

| 角色名称                        | 说明                                               |
|---------------------------------|----------------------------------------------------|
| Owner                           | 拥有所有权限，包括分配权限                         |
| Contributor                     | 拥有除分配权限外的所有管理权限                     |
| Reader                          | 只读所有资源                                      |
| User Access Administrator       | 管理用户对资源的访问权限                          |
| Virtual Machine Contributor     | 管理虚拟机，但不能删除                            |
| Storage Account Contributor     | 管理存储账户，但不能访问密钥                       |
| Storage Blob Data Contributor   | 管理和访问 Blob 数据                              |
| Storage Blob Data Reader        | 只读 Blob 数据                                    |
| Storage Queue Data Contributor  | 管理和访问队列数据                                |
| Storage File Data SMB Share Contributor | 管理 SMB 文件共享数据                    |
| Network Contributor             | 管理网络资源                                      |
| Security Admin                  | 管理安全相关配置                                  |
| Monitoring Reader               | 只读监控和诊断数据                                |
| Log Analytics Reader            | 只读 Log Analytics 工作区                         |
| SQL DB Contributor              | 管理 SQL 数据库，但不能管理服务器                 |
| Key Vault Contributor           | 管理 Key Vault 配置，但不能访问密钥/机密           |
| Key Vault Secrets User          | 读取 Key Vault 中的机密                           |
| Managed Identity Contributor    | 管理托管身份                                      |
| Function App Contributor        | 管理 Function App                                 |
| Web Plan Contributor            | 管理 App Service 计划                             |
| App Service Contributor         | 管理 App Service                                  |

**完整列表可参考官方文档**：[Azure 内置角色一览](https://learn.microsoft.com/zh-cn/azure/role-based-access-control/built-in-roles)

> 除上述常用角色外，还有针对特定服务和场景的数十种内置角色，支持最小权限原则和细粒度授权。