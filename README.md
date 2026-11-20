# GhostLink 定制版 Mihomo

本仓库只分发经过 GhostLink 专项测试和完整 Mihomo Release 构建流程验证的
**预编译 Mihomo 核心**。源码仓库保持私有，公开仓库不包含服务器源码、许可证
私钥或任何共享密钥。

当前版本支持原生 `type: ghostlink`，并包含无需设置
`GODEBUG=http2xconnect=1` 的 `h2-capsule` 支持。五个平台产物和统一
`SHA256SUMS` 只从通过测试的构建任务发布。

## 下载与校验

在 [Releases](https://github.com/AresQQQQQ/ghostlink-client/releases) 下载对应
平台的 `mihomo-<版本>-ghostlink-<平台>-<架构>` 文件和 `SHA256SUMS`，在同一
目录校验：

```bash
sha256sum -c SHA256SUMS
```

macOS 可使用：

```bash
shasum -a 256 -c SHA256SUMS
```

## Windows

下载 `mihomo-v*-ghostlink-windows-amd64.exe`，将其改名或直接作为
`mihomo.exe` 使用。先在结果目录创建 `config.yaml`，再运行：

```powershell
.\mihomo.exe -d . -f .\config.yaml
```

如使用支持自定义核心的图形客户端，请在客户端设置中将 Mihomo 核心路径指向该
文件。

## macOS

下载 `mihomo-v*-ghostlink-darwin-arm64`（Apple Silicon）或
`mihomo-v*-ghostlink-darwin-amd64`（Intel），赋予执行权限并移除下载隔离属性：

```bash
chmod +x mihomo-v*-ghostlink-darwin-*
xattr -cr mihomo-v*-ghostlink-darwin-*
codesign --force --deep --sign - mihomo-v*-ghostlink-darwin-*
./mihomo-v*-ghostlink-darwin-arm64 -d . -f ./config.yaml
```

Intel 机器请将最后一行改为 `darwin-amd64`。

## Linux

下载 `mihomo-v*-ghostlink-linux-amd64` 或
`mihomo-v*-ghostlink-linux-arm64`，根据服务器或设备架构选择：

```bash
chmod +x mihomo-v*-ghostlink-linux-*
./mihomo-v*-ghostlink-linux-amd64 -d . -f ./config.yaml
```

## OpenWrt / OpenClash

1. 根据设备架构选择 `linux-arm64`、`linux-amd64` 等产物。
2. 在 OpenClash 中暂停运行，备份原核心。
3. 将新核心替换为 `/etc/openclash/core/clash_meta`，保留可执行权限。
4. 在 OpenClash 的 Zashboard 面板中刷新，确认订阅单节点测速可用。

```bash
/etc/init.d/openclash stop
cp /etc/openclash/core/clash_meta /etc/openclash/core/clash_meta.bak
cp mihomo-v*-ghostlink-linux-arm64 /etc/openclash/core/clash_meta
chmod 0755 /etc/openclash/core/clash_meta
/etc/init.d/openclash start
```

替换前先用 `uname -m` 确认架构。路由器空间紧张时，请在上传后保留足够的临时空间。

## 配置示例

```yaml
proxies:
  - name: ghostlink-node
    type: ghostlink
    server: panel.example.com
    port: 443
    client-id: example-client
    password: example-secret
```

具体字段以 `3x-ui-ghostlink` 面板生成的订阅或分享链接为准。

## 相关项目

- [GhostLink 3x-ui 面板](https://github.com/AresQQQQQ/3x-ui-ghostlink)
- [GhostLink 3x-ui 一键部署与授权说明](https://github.com/AresQQQQQ/3x-ui-ghostlink/blob/main/README.GhostLink.md)

本仓库的 Release 只包含预编译客户端核心。面板镜像与许可证服务分别部署，客户端
不包含服务器私钥或共享分发密钥。
