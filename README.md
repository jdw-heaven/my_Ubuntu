## 将 WSL2 发行版迁移至其他目录

### 第一步：关闭 WSL

```bash
wsl --shutdown
```

> 确保所有正在运行的 WSL 实例已完全停止。

---

### 第二步：导出 Linux 发行版

```bash
wsl --export Ubuntu D:/export.tar
```

将目标发行版打包导出为 `.tar` 文件，保存至指定路径。

---

### 第三步：注销原有发行版

```bash
wsl --unregister Ubuntu
```

> ⚠️ 此操作会删除原注册信息，请确保导出文件已完整保存。

---

### 第四步：导入至新目录

```bash
wsl --import Ubuntu D:\export\ D:\export.tar --version 2
```

将导出的 `.tar` 文件导入到新的存储路径，并指定使用 WSL2。

---


## Proxy

添加下述代码至`.bashrc`

```
# proxy
# host_ip is the proxy address
host_ip=127.0.0.1:6478
export http_proxy="http://$host_ip"
export https_proxy="http://$host_ip"
export all_proxy="socks5://$host_ip"
```

## 记录整个终端会话（最常用）

```bash
script log.txt

exit    # 结束时输入会生成 log.txt
```

## 导出历史命令（不是输出）

```bash
history > history.txt
```