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

## 克隆 my_Ubuntu repo

```bash
# 设置默认 id

git config --global user.email "jdw.heaven@gmail.com"
git config --global user.name "jdw-heaven"

git clone https://github.com/jdw-heaven/my_Ubuntu.git
```



## 记录终端会话

在 `.bashrc` 中添加：

```bash
# ---------- history 基本设置 ----------
HISTSIZE=50000
HISTFILESIZE=100000
# 减少重复
# HISTCONTROL=ignoredups:erasedups
# 忽略没必要的命令
# HISTIGNORE="ls:bg:fg:history:clear:exit"

shopt -s histappend
shopt -s cmdhist

HISTTIMEFORMAT="%F %T "

# 历史文件位置
HISTFILE="$HOME/.bash_history"

# 每次显示提示符前：
# 1. 先把当前 session 新命令写入 HISTFILE
# 2. 再把其他终端刚写入的命令读进来
PROMPT_COMMAND="history -a; history -n; $PROMPT_COMMAND"

# ---------- 退出时按天归档 ----------
__save_daily_history() {
    local dayfile="$HOME/my_Ubuntu/my_logs/$(date +%Y%m%d)_bash.txt"

    # 先确保当前会话 history 已写入主历史文件
    history -a
    history -n

    # 导出当前 shell 可见的 history
    history | sed 's/^[ ]*[0-9]\+[ ]*//' > "$dayfile"
}

trap '__save_daily_history' EXIT

# ---------- script ----------
if [ -t 1 ] && [ -z "$SCRIPT_LOGGING" ]; then
    export SCRIPT_LOGGING=1

    logdir="$HOME/my_Ubuntu/my_logs/full"
    mkdir -p "$logdir"

    ts="$(date +%Y%m%d_%H%M%S)"
    base="${ts}_$(hostname)_$$"

    logfile="$logdir/${base}.typescript"
    timingfile="$logdir/${base}.timing"

    exec script -q -f --timing="$timingfile" "$logfile"
fi
```


## 查看 Linux 发行版本信息

```bash
cat /etc/os-release
```

## git push 操作

```bash
git add .
git commit -m "update"
git push
```