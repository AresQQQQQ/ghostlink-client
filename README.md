> **GhostLink 客户端核心**
>
> 下载适用于 Windows、macOS、Linux 和 OpenWrt 的预编译核心。

# GhostLink 客户端核心

## 项目内容

本仓库公开分发 GhostLink 定制版 **Mihomo 核心程序**，用于连接由
`3x-ui-ghostlink` 面板创建的节点。

这里提供的是核心文件，并非带界面的完整客户端。使用桌面图形界面时，需要选择
支持替换本地 Mihomo 核心的客户端。OpenClash 用户则替换现有 `clash_meta`
文件。

Release 包含：

- Windows amd64；
- macOS arm64、amd64；
- Linux amd64、arm64；
- 一份 `SHA256SUMS` 校验文件。

## 与上游 Mihomo 的关系

本项目基于 [MetaCubeX/mihomo](https://github.com/MetaCubeX/mihomo)，增加
GhostLink 节点支持，版本号会保留上游版本，例如 `v1.19.34-ghostlink`。

这是本项目的非官方构建，并非 MetaCubeX/mihomo 官方 Release。本仓库只分发
预编译核心和使用说明，定制源码保持私有。

## 快速使用

先在服务器完成 [3x-ui-ghostlink 部署与授权](https://github.com/AresQQQQQ/3x-ui-ghostlink/blob/main/README.GhostLink.md)。

然后：

1. 在 3x-ui 面板创建 GhostLink 入站和用户；
2. 复制面板生成的订阅或分享链接；
3. 从本仓库 Releases 下载适合设备的 Mihomo 核心；
4. 将核心配置到图形客户端或 OpenClash；
5. 导入订阅或分享链接并启用。

### 选择下载文件

| 设备或系统 | 下载文件 |
| --- | --- |
| Windows 64 位 | `mihomo-*-windows-amd64.exe` |
| macOS Apple Silicon | `mihomo-*-darwin-arm64` |
| macOS Intel | `mihomo-*-darwin-amd64` |
| Linux x86_64 | `mihomo-*-linux-amd64` |
| Linux ARM64 | `mihomo-*-linux-arm64` |

### Windows

下载 `windows-amd64` 文件。如果图形客户端允许选择本地 Mihomo 核心，在客户端
设置中将核心路径指向该文件，然后导入面板订阅。

如果客户端不允许替换本地核心，就不能直接使用本仓库的内核；请换用支持本地
核心的客户端，或使用命令行方式运行。

### macOS

Apple Silicon 下载 `darwin-arm64`，Intel 下载 `darwin-amd64`。在终端进入下载
目录后执行：

```bash
chmod +x mihomo-*-ghostlink-darwin-*
xattr -cr mihomo-*-ghostlink-darwin-*
codesign --force --deep --sign - mihomo-*-ghostlink-darwin-*
```

然后在支持本地 Mihomo 核心的图形客户端中选择该文件，并导入面板订阅。

这里执行的是 macOS 本地临时签名，不代表 Apple Developer 身份签名。

### Linux

根据系统架构下载 `linux-amd64` 或 `linux-arm64`，然后赋予执行权限：

```bash
chmod +x mihomo-*-ghostlink-linux-*
```

随后在支持 Mihomo 的客户端或命令行环境中加载该文件，并导入面板订阅。

### OpenWrt / OpenClash

先执行 `uname -m` 确认路由器架构，再备份并替换 OpenClash 的
`/etc/openclash/core/clash_meta`：

```bash
/etc/init.d/openclash stop
cp /etc/openclash/core/clash_meta /etc/openclash/core/clash_meta.bak
cp mihomo-*-ghostlink-linux-arm64 /etc/openclash/core/clash_meta
chmod 0755 /etc/openclash/core/clash_meta
/etc/init.d/openclash start
```

示例对应常见的 ARM64 路由器。其他架构请换成对应文件。

Zashboard 的单节点测速是网页面板功能，需要 OpenClash 已安装支持该功能的
Zashboard 版本；替换 Mihomo 核心不会升级网页面板。

## 下载校验

将平台文件与 `SHA256SUMS` 放在同一目录。

Linux：

```bash
sha256sum -c SHA256SUMS
```

macOS：

```bash
shasum -a 256 -c SHA256SUMS
```

当前发布提供 SHA-256 校验，不对应 Windows Authenticode 或 Apple Developer
身份签名。

## 两个仓库的关系

| 仓库 | 使用位置 | 作用 |
| --- | --- | --- |
| [`3x-ui-ghostlink`](https://github.com/AresQQQQQ/3x-ui-ghostlink) | 服务器 | 创建节点、申请授权、生成订阅或分享链接 |
| [`ghostlink-client`](https://github.com/AresQQQQQ/ghostlink-client) | 用户设备 | 提供能够连接 GhostLink 节点的 Mihomo 核心 |

面板负责服务器端配置和授权，客户端核心负责用户设备连接。用户不需要在客户端
填写服务器端授权信息。
