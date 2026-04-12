# 📦 ImmortalWrt 360 V6 增强版 (Enhanced)

# OpenWrt 固件 - 360 V6 (Qualcomm IPQ60xx)

基于 ImmortalWrt master 分支自动编译，专为 360 V6 路由器定制。

---

## 硬件规格

| 项目 | 参数 |
|---|---|
| 型号 | 360 V6 |
| SoC | Qualcomm IPQ6010，四核 Cortex-A53 @ 1GHz |
| RAM | 512MB |
| Flash | 128MB NAND |
| 无线 | AX1800，2.4G + 5G 双频 Wi-Fi 6 |
| 有线 | 4 × 千兆 LAN + 1 × 千兆 WAN |
| USB | USB 3.0 × 1 |

---

## 固件特性

### 🌐 界面与主题
- 中文界面
- Argon 主题（默认）+ Bootstrap 主题备选

### 🔒 代理与安全
- **HomeProxy + sing-box**：现代化代理平台，支持多种协议
- **AdGuard Home**：全局广告与追踪过滤，DNS 级拦截
- **SmartDNS**：国内 DNS 加速，可作为 AdGuard Home 上游

### 🌍 网络功能
- **DDNS 动态域名**：内置 Cloudflare 脚本，公网 IP 变化自动更新
- **UPnP**：游戏、BT 自动端口映射（nftables 版）
- **IPv6 完整支持**：含 IPv6 Helper 一键配置界面
- **BBR 拥塞控制**：开机自动生效，弱网环境提升吞吐量

### 💾 存储与共享
- **USB 自动挂载**：插入即用，无需手动操作
- **文件系统支持**：ext4、NTFS3（内核原生，速度快）
- **DiskMan**：磁盘管理与分区界面
- **Samba4 文件共享**：USB 硬盘共享至局域网，简单 NAS
- **smartmontools**：USB 硬盘健康检测，查看寿命与坏道
- **文件管理器**：luci-app-fileassistant，浏览器直接管理文件

### 📊 监控与管理
- **流量监控**：nlbwmon，按设备统计用量
- **定时重启**：可设置每天固定时间自动重启
- **计划任务**：crontabs 图形界面，无需手写 cron
- **网络唤醒**：WOL，远程唤醒局域网设备
- **ttyd 网页终端**：浏览器直接 SSH，无需额外工具

### 🔧 诊断工具
- **mtr**：网络路径追踪与延迟分析
- **tcpdump-mini**：轻量抓包工具
- **htop**：系统资源实时监控

---

## 默认配置

| 项目 | 默认值 |
|---|---|
| 管理地址 | http://192.168.2.1 |
| 用户名 | root |
| 密码 | 无（首次登录请立即设置） |
| IPv6 | 已内置，默认关闭，按需开启 |
| BBR | 开机自动启用 |

---

## 首次使用指引

### 第一步：刷入固件
使用路由器原厂 Web 界面或 U-Boot 刷入 `sysupgrade` 固件文件。

### 第二步：连接路由器
浏览器访问 `http://192.168.2.1`，用户名 `root`，密码留空直接登录。

### 第三步：必做安全设置
```
系统 → 管理权 → 设置 root 密码
```

### 第四步：配置 Wi-Fi
```
网络 → 无线 → 修改 SSID 和密码 → 保存并应用
```

### 第五步：按需开启功能

**开启 AdGuard Home：**
```
服务 → AdGuard Home → 启用 → 保存并应用
```

**开启 IPv6：**
```
服务 → IPv6 Helper → 启用 → 保存并应用
```

**配置 Samba 共享（需插入 USB 硬盘）：**
```
服务 → 网络共享 → 添加共享路径 → 保存并应用
```

**配置 DDNS：**
```
服务 → 动态 DNS → 选择服务商（如 Cloudflare）→ 填入域名和 API Token
```

**配置 HomeProxy：**
```
服务 → HomeProxy → 添加节点 → 开启代理
```

---

## 空间使用参考

| 分区 | 大小 | 说明 |
|---|---|---|
| /rom | ~38MB | 固件只读分区（squashfs 压缩后） |
| /overlay | ~36MB | 可写分区，用于运行时修改 |
| 合计 Flash | 128MB | NAND 总容量 |

overlay 剩余空间充裕，可通过 `apk add` 额外安装软件包。

---

## 常用命令

```bash
# 查看磁盘空间
df -h

# 更新软件包列表
apk update

# 安装软件包
apk add <包名>

# 查看 USB 硬盘健康
smartctl -a /dev/sda

# 验证 BBR 是否生效
sysctl net.ipv4.tcp_congestion_control

# 查看实时流量
ifstat

# 重启路由器
reboot
```

---

## 自动编译说明

本固件通过 GitHub Actions 自动编译，每次推送代码自动触发。

| 项目 | 内容 |
|---|---|
| 源码 | ImmortalWrt master 分支 |
| 编译环境 | Ubuntu 22.04 |
| 触发方式 | 推送 `.config` 或 workflow 文件时自动触发，也可手动触发 |
| 产物 | Release 页面下载 sysupgrade 固件 |

---

## 注意事项

- 固件基于 ImmortalWrt **master（SNAPSHOT）** 分支，为滚动更新版本，功能最新但非长期稳定版
- 刷机前请备份原厂固件
- Samba 共享性能受限于 USB 3.0 及 CPU，适合日常文件传输，不适合高并发读写
- SmartDNS 与 AdGuard Home 建议配合使用：SmartDNS 作为 AdGuard Home 的上游 DNS
- IPv6 功能是否可用取决于运营商是否开通，开通前请联系运营商

---

## 相关链接

- [ImmortalWrt 官方仓库](https://github.com/immortalwrt/immortalwrt)
- [HomeProxy 仓库](https://github.com/immortalwrt/homeproxy)
- [Argon 主题仓库](https://github.com/jerrykuku/luci-theme-argon)
- [AdGuard Home 官网](https://adguard.com/zh_cn/adguard-home/overview.html)
- [OpenWrt 硬件数据库 - 360 V6](https://openwrt.org/toh/qihoo/360_v6)
