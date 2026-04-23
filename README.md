# YAT Client Releases

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/tunnel6/yat-client)](https://github.com/tunnel6/yat-client/releases/latest)

[English](README_EN.md) | [中文](README.md)

YAT (Yet Another Tunnel) 桌面客户端发布仓库 - 提供多平台安装包下载

**官方网站**: [https://www.tunnel6.com](https://www.tunnel6.com)  
**文档**: [https://yat.github.io](https://yat.github.io)  
**主仓库**: [https://github.com/tunnel6/yat](https://github.com/tunnel6/yat)

## 简介

YAT 是一个简单安全的内网穿透解决方案，提供零信任安全架构、高性能隧道传输和可扩展的插件生态系统。本仓库专门用于发布 YAT 桌面客户端 (yat-fe) 的构建产物。

## 特性

- 🔒 **零信任安全**: mTLS 双向认证，端到端加密
- 🚀 **高性能**: 支持 P2P 直连、WireGuard 协议，超低延迟
- 🌍 **全球部署**: 边缘节点全球分布，智能路由优化
- 🔌 **可扩展**: 丰富的扩展生态，支持 Vue/React 开发自定义应用
- 🎯 **易于使用**: 精美的桌面客户端，一键部署，零配置

## 支持平台

| 平台 | 架构 | 安装包格式 |
|------|------|-----------|
| **Windows** | x64, ARM64 | `.exe` 安装程序 |
| **macOS** | Intel (x64), Apple Silicon (ARM64) | `.dmg`, `.zip` |
| **Linux** | amd64, arm64 | `.deb`, `.AppImage` |

## 下载

### 最新版本

访问 [GitHub Releases](https://github.com/tunnel6/yat-client/releases/latest) 下载最新版本。

### 所有版本

查看 [所有发布版本](https://github.com/tunnel6/yat-client/releases) 获取历史版本和更新日志。

## 安装指南

### Windows

1. 下载 `.exe` 安装程序
2. 双击运行安装向导
3. 按照提示完成安装

### macOS

1. 下载 `.dmg` 文件
2. 打开 DMG 文件
3. 将 YAT 拖拽到 Applications 文件夹

> **注意**: 首次运行可能需要在"系统偏好设置 > 安全性与隐私"中允许打开应用

### Linux (Debian/Ubuntu)

```bash
# 下载 .deb 安装包
wget https://github.com/tunnel6/yat-client/releases/latest/download/yat_x.x.x_amd64.deb

# 安装
sudo dpkg -i yat_x.x.x_amd64.deb

# 如果有依赖问题，运行
sudo apt-get install -f
```

### Linux (AppImage)

```bash
# 下载 AppImage
wget https://github.com/tunnel6/yat-client/releases/latest/download/yat_x.x.x_x86_64.AppImage

# 添加执行权限
chmod +x yat_x.x.x_x86_64.AppImage

# 运行
./yat_x.x.x_x86_64.AppImage
```

## 使用

### 快速开始

1. **启动应用**: 从应用程序菜单或命令行启动 YAT
2. **登录账户**: 使用您的 YAT 账户登录（或注册新账户）
3. **创建隧道**: 点击"新建隧道"按钮，配置本地服务
4. **访问服务**: 使用生成的公网地址访问您的本地服务

### 主要功能

- **HTTP/HTTPS 隧道**: 暴露本地 Web 服务到公网
- **TCP/UDP 隧道**: 支持任意 TCP/UDP 服务穿透
- **WireGuard VPN**: 构建安全的虚拟专用网络
- **远程桌面**: 通过扩展支持 ARD/VNC 远程桌面
- **扩展系统**: 安装自定义扩展增强功能


## 更新

YAT 客户端支持自动更新：

- **macOS/Linux**: 应用启动时自动检查更新
- **Windows**: 通过安装程序自动更新

您也可以手动检查更新：帮助菜单 > 检查更新

## 反馈与支持

- 📖 **文档**: [https://yat.github.io/guide/](https://yat.github.io/guide/)
- 🌐 **官网**: [https://www.tunnel6.com](https://www.tunnel6.com)
- 🐛 **问题反馈**: [GitHub Issues](https://github.com/tunnel6/yat/issues)
- 💬 **讨论**: [GitHub Discussions](https://github.com/tunnel6/yat/discussions)
- 📧 **邮件**: support@tunnel6.com

## 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件

## 贡献

我们欢迎社区贡献！如果您想参与开发，请访问 [主仓库](https://github.com/tunnel6/yat) 了解更多信息。

## 相关链接

- **官方网站**: [https://www.tunnel6.com](https://www.tunnel6.com)
- **文档**: [https://www.tunnel6.com/docs](https://www.tunnel6.com/zh/docs)
- **主仓库**: [https://github.com/tunnel6/yat](https://github.com/tunnel6/yat)
- **用户指南**: [https://yat.github.io/guide/](https://yat.github.io/guide/)
- **下载页面**: [https://yat.github.io/download](https://yat.github.io/download)
- **扩展开发指南**: [https://yat.github.io/developer/](https://yat.github.io/developer/)
