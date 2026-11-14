# OpenYAT

[![License: Apache-2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0)

[English](README.md) | [中文](README_ZH.md)

OpenYAT 是 YAT（Yet Another Tunnel）的开源客户端与私有化组网方案。它以自建 **Edge** 为运行时协调节点，通过 mTLS 完成设备认证，提供内网穿透、TCP/UDP/HTTP 转发和 WireGuard 私有网络能力。

OpenYAT 运行时不需要平台账号。设备在本地生成身份与密钥，使用一次性配对令牌或网络邀请码向 Edge 申请设备证书；后续通信由 Edge 自签 CA 验证并使用 mTLS 加密。

## 核心能力

- **私有化部署**：Edge 可部署在自有 VPS、云主机或具有公网可达性的服务器上，数据与运行时状态由用户掌控。
- **零信任连接**：客户端与 Edge 使用双向证书认证，设备身份、Owner/Device 权限和网络授权均由证书链约束。
- **内网穿透**：支持 HTTP/HTTPS、TCP、UDP 服务的公网暴露，适合 NAS、开发环境、内网数据库和 IoT 设备等场景。
- **WireGuard 组网**：通过本地 WireGuard adapter 建立设备间虚拟网络，支持直连优先、Edge 中继回退以及强制中继等路径策略。
- **多端客户端**：桌面端面向 macOS、Windows 和 Linux；`yat-fe-linux` 面向 Linux 服务器、嵌入式和自动化场景提供 CLI/daemon 使用方式。
- **可扩展运行时**：客户端与 Edge 之间采用协议化的 tunnel/channel 状态同步，业务状态由 Edge 统一协调并向订阅设备收敛。

## 架构总览

OpenYAT 将业务层、协议传输层和本地网络执行层拆分为独立项目。核心依赖关系如下：

```text
                 ┌───────────────┐
                 │     Edge      │
                 │  边缘运行时    │
                 └──┬─────────▲──┘
                    │         │
                mTLS / 协议同步
                    │         │
        ┌───────────▼─────────┴───────────┐
        │  OpenYAT 桌面端 / yat-fe-linux  │
        └───────────────┬─────────────────┘
                        │
                        ▼
                  ┌───────────┐
                  │  network  │
                  │ 客户端核心 │
                  └────┬──┬───┘
                       │  │
          ┌────────────┘  └────────────┐
          ▼                           ▼
   ┌────────────┐              ┌────────────┐
   │ transport  │              │  wireguard │
   └────────────┘              └────────────┘
```

更简洁地表达为：

```text
Edge <──> OpenYAT / yat-fe-linux
OpenYAT / yat-fe-linux -> network -> { transport | wireguard }
```

### 运行时职责

| 层级 | 项目 | 职责 |
| --- | --- | --- |
| 服务端 | [Edge](https://github.com/tunnel6/edge.git) | 边缘运行时和隧道协调器；维护设备会话、证书签发、channel 状态、网络元数据、WireGuard relay 和客户端同步事件。 |
| 客户端业务层 | [network](https://github.com/tunnel6/network.git) | 客户端核心业务处理；负责配对、证书申请、网络/隧道状态聚合、本机运行时配置和 Electron/CLI 调用桥接。 |
| 协议传输层 | [transport](https://github.com/tunnel6/transport.git) | Edge 与客户端之间的协议传输；承载 mTLS 会话、FlatBuffers 消息、隧道控制和状态同步。 |
| 本地 WG 执行层 | [wireguard](https://github.com/tunnel6/wireguard.git) | WireGuard 协议包装与 WG 业务控制层；管理本机密钥、peer、AllowedIPs、接口、路由和运行状态。 |
| 桌面客户端 | OpenYAT | Electron + Vue 桌面客户端；提供网络、隧道、Edge、配对和运行状态的管理界面。 |
| Linux CLI | [yat-fe-linux](https://github.com/tunnel6/yat-fe-linux.git) | Linux client CLI 版本；适合服务器、无桌面环境、systemd 服务和自动化运维场景。 |

## 关联项目

| 项目 | 地址 | 说明 |
| --- | --- | --- |
| Edge | <https://github.com/tunnel6/edge.git> | Edge 边缘服务，是 OpenYAT 的运行时协调和公网接入节点。 |
| network | <https://github.com/tunnel6/network.git> | 客户端核心业务处理，是桌面端和 Linux CLI 复用的业务层。 |
| transport | <https://github.com/tunnel6/transport.git> | Edge 与客户端之间的协议传输实现。 |
| wireguard | <https://github.com/tunnel6/wireguard.git> | WireGuard 协议包装与 WG 业务控制层。 |
| yat-fe-linux | <https://github.com/tunnel6/yat-fe-linux.git> | Linux client CLI 版本。 |

## 运行流程

1. **部署 Edge**：在公网可达的服务器上启动 Edge。首次启动时，Edge 生成自己的 CA 与服务证书。
2. **配对设备**：Edge 管理员生成一次性 Owner 或 Device 配对令牌；客户端使用该令牌连接 Edge 并申请设备证书。
3. **建立 mTLS 会话**：客户端保存设备身份后，后续 tunnel 控制、channel 管理和状态同步均通过 mTLS 会话完成。
4. **创建或加入网络**：Owner 创建 WireGuard 网络，其他设备通过 invite 包加入。分享内容只包含邀请凭据和公开 CA，不导出客户端私钥。
5. **同步业务状态**：`network` 通过 `transport` 与 Edge 同步网络、隧道和 channel 状态；`wireguard` 负责把网络配置落到本机接口、路由和 peer 运行时。
6. **选择数据路径**：设备间通信优先尝试直连；直连不可用时，可按网络配置切换到 Edge relay。

## 安装

### 桌面端

前往 [GitHub Releases](https://github.com/tunnel6/yat/releases) 下载对应平台安装包。

| 平台 | 架构 | 安装包 |
| --- | --- | --- |
| Windows | x64 / ARM64 | `.exe` |
| macOS | x64 / ARM64 | `.dmg`, `.zip` |
| Linux | amd64 / arm64 | `.deb`, `.AppImage` |

#### Windows

1. 下载 `.exe` 安装程序。
2. 双击运行并按安装向导完成安装。

#### macOS

1. 下载 `.dmg` 文件并打开。
2. 将 OpenYAT 拖入 Applications 文件夹。
3. 首次启动如被 Gatekeeper 拦截，请在「系统设置 > 隐私与安全性」中允许打开。

#### Linux

```bash
# Debian / Ubuntu
sudo dpkg -i openyat_<version>_amd64.deb

# 如遇依赖问题
sudo apt-get install -f
```

```bash
# AppImage
chmod +x openyat_<version>_x86_64.AppImage
./openyat_<version>_x86_64.AppImage
```

> 文件名中的 `<version>` 需替换为实际下载的版本号。

### Linux CLI

在服务器或无桌面环境中，推荐使用 [yat-fe-linux](https://github.com/tunnel6/yat-fe-linux)。常用流程如下：

```bash
# 配对 Edge
yat-client pair --edge example.com:9443 --token <PAIR_TOKEN>

# 导入网络邀请包
yat-client import <INVITE_FILE>
```

具体命令参数请以 `yat-fe-linux` 仓库中的 README 和 CLI 帮助为准。

## 快速开始

1. 启动 OpenYAT 桌面端或 `yat-client`。
2. 输入 Edge 地址和一次性配对令牌，完成设备配对。
3. Owner 设备创建 WireGuard 网络。
4. 导出网络 invite，并在其他设备导入。
5. 在网络成员页确认设备在线、IP 分配和连接路径。
6. 按需创建 HTTP/HTTPS、TCP 或 UDP tunnel，并通过 Edge 返回的地址访问内网服务。

## 安全模型

- **设备身份**：客户端本地生成设备密钥，不依赖平台账号密码。
- **Edge 自治 CA**：每个 Edge 拥有自己的证书信任根，客户端按 Edge 保存对应 CA。
- **一次性授权**：配对令牌和网络 invite 一次性消费，避免长期共享密钥。
- **权限隔离**：Owner、Device 和 invite scope 由证书扩展与 Edge 会话共同约束，客户端请求不能伪造身份或扩大授权。
- **最小化分享**：网络分享仅包含 invite 与公开 CA，不导出客户端证书私钥。
- **传输加密**：业务控制与状态同步使用 mTLS；WireGuard 数据面使用其原生加密协议。

## 文档

- YAT 使用指南：<[https://tunnel6.com/guide/](https://tunnel6.com/zh/guide/what-is-yat.html)>
- Edge 部署与配置：<[https://tunnel6.com/guide//guide/edge/deploy.html](https://tunnel6.com/zh/guide/edge/deploy.html)>
- WireGuard 组网：<[https://tunnel6.com/guide/guide/wireguard-networking.html](https://tunnel6.com/zh/guide/wireguard-networking.html)>
- 传输模式：<https://tunnel6.com/zh/guide/transport-modes.html>
- GitHub Issues：<https://github.com/tunnel6/yat/issues>

## 贡献

欢迎通过 Issue 反馈问题，或向上述各项目提交 Pull Request。提交前请阅读对应仓库的构建与测试说明，并确保 Edge、network、transport、wireguard 和客户端二进制没有混用不兼容版本。

## License

OpenYAT 客户端采用 Apache License 2.0。各关联项目许可证以其仓库中的 LICENSE 文件为准。
