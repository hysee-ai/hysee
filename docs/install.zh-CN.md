# 安装

当前平台：**macOS 13 Ventura 及以上，Apple Silicon**。

[English](install.md)

## 下载应用

1. 打开 [https://hysee.ai](https://hysee.ai)。
2. 下载 macOS Apple Silicon 安装包（`HYSEE-<version>-arm64.dmg`）。
3. 把 **HYSEE.app** 拖进 `/Applications`。

请从产品站获取安装包。本仓库不托管发布二进制。

## 在 macOS 上第一次打开

HYSEE 使用 ad-hoc 签名。Gatekeeper 可能拦住首次启动。

- 右键 **HYSEE.app** → 打开 → 确认，或
- 清掉隔离标记：

```bash
xattr -cr /Applications/HYSEE.app
```

## 第一个 Agent

创建或启动 Agent 时，若本地还没有客机 rootfs，会下载一次，放到：

```text
~/Library/Application Support/HYSEE/
```

这份缓存是持久的。之后的 Agent 会复用。只打开 App、舱还没跑起来时，不会去下 rootfs。

某类 Agent 第一次就绪需要能访问网络（CLI 会装进共享的 agent-store）。之后可以从缓存启动。

## 你需要什么

- Apple Silicon 的 Mac
- macOS 13 Ventura 及以上
- 足够放下缓存 rootfs 的磁盘（大约一份 Linux 用户态，不是瘦身 dmg 那么小）

**不需要** Homebrew、Docker，也不需要本机 Go 工具链才能用这个 App。

## Homebrew

产品打包里有 Cask 公式（`hysee`，文件名 `HYSEE-<version>-arm64.dmg`）。公开 tap 还不是主安装路径。在 tap 发布之前，请从 [hysee.ai](https://hysee.ai) 下载。

## Linux 与 Windows

今天还没有可下载的安装包。运行时设计里有 Linux Firecracker 路径和 Windows 路径，在路线图上。

## 卸载

删除 **HYSEE.app**，可选再删：

```text
~/Library/Application Support/HYSEE
~/Library/Logs/HYSEE
~/Library/Preferences/ai.hysee.desktop.plist
```
