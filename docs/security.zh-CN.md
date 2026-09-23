# 安全模型

海视"HYSEE"把 Agent 当成会碰文件、也会碰网络的不可信代码。海视"HYSEE"产品是一间 **水晶舱**：先隔离，再让你看见，再管出站。

[English](security.md)

## 主要特性

### 隔离

在 macOS Apple Silicon 上，舱是 **libkrun MicroVM**，与宿主机内核是完全隔离的。客机全都运行 Linux。当前路径没有 virtio-net 网卡。出站流量通过 HTTP(S) 代理到宿主上的网关。

Agent 拿不到你的 `$HOME`。只有绑定进去的路径内容可见，通常在 `/work/<basename>`。Reset 或 Destroy 只清舱内草稿，绝不删宿主绑定。

### 可视

玻璃盒是安全的另一半。Timeline、Diff、Terminal 让你看见执行、改文件、网络裁决，而不用去跟踪一份原始日志。这就是「Safe Agents / 智能体极致安全舱」的可视一面：舱是能看见的。

### 出站

每个 Agent 有一份 allowlist（Claude Code、Codex、OpenCode、Hermes、OpenClaw 带预设）。支持 `*.opencode.ai` 这类通配。未知目的地会被拒绝。

剪贴板、客机写文件、舱内执行也可以按策略弹出 guard 事件。

## 这套模型针对什么

- Agent 跑 `rm`、`npm install` 或其它不该落到宿主机的 shell
- Prompt injection 想从宿主机Home目录读密钥（除非你自己把那些路径绑进舱，否则客机里没有）
- Agent 向你没允许的主机发送网络请求

## 现在还不是什么

slogan 不是一份已经做完的合规套件。对照要写清楚。

| 容易脑补的能力 | 现在 |
| --- | --- |
| Agent 进程在硬件隔离的客机里 | 是，macOS Apple Silicon（libkrun） |
| 不绑定则看不见宿主家目录 | 是 |
| 默认拒绝出站，allowlist + Ask | 是 |
| 完整 DLP：正文脱敏、一键放行并导出审计 | 还不是完整产品面 |
| 快照、休眠、回滚内存态 | 未交付（Stop 会丢掉 RAM） |
| Apple Developer ID 签名 / 公证 | ad-hoc 签名。Gatekeeper 要多点一次 |
| 同等边界的 Linux / Windows 安装包 | 尚无可用的下载安装包 |
| 把舱内 Docker 当作嵌套隔离层 | 未交付 |

如果你绑定的目录里已经有 `.env` 或密钥，Agent 就能读。舱不会自动洗掉你主动共享的文件。

## 使用时要注意

- allowlist 本身就是威胁模型的一部分。过大的 `*` 就是洞。
- Destroy 删除整间舱。Stop 只关掉 MicroVM。
- rootfs 缓存在 `~/Library/Application Support/HYSEE/`。那是客机用户态，不是密钥库，卸载时可以删。

## 相关文档

- [架构](architecture.zh-CN.md)
- [安装](install.zh-CN.md)
