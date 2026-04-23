# YAT Client Releases

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/tunnel6/yat)](https://github.com/tunnel6/yat-client/releases/latest)

[English](README_EN.md) | [中文](README.md)

YAT (Yet Another Tunnel) Desktop Client Release Repository - Multi-platform installation packages

**Official Website**: [https://www.tunnel6.com](https://www.tunnel6.com)  
**Documentation**: [https://www.tunnel6.com/docs](https://www.tunnel6.com/docs)  
**Main Repository**: [https://github.com/tunnel6/yat](https://github.com/tunnel6/yat)

## Introduction

YAT is a simple and secure intranet penetration solution, providing zero-trust security architecture, high-performance tunnel transmission, and an extensible plugin ecosystem. This repository is dedicated to releasing build artifacts for the YAT desktop client (yat-fe).

## Features

- 🔒 **Zero-Trust Security**: mTLS mutual authentication, end-to-end encryption
- 🚀 **High Performance**: P2P direct connection, WireGuard protocol support, ultra-low latency
- 🌍 **Global Deployment**: Edge nodes worldwide, intelligent routing optimization
- 🔌 **Extensible**: Rich extension ecosystem, support Vue/React for custom app development
- 🎯 **Easy to Use**: Beautiful desktop client, one-click deployment, zero configuration

## Supported Platforms

| Platform | Architecture | Package Format |
|----------|-------------|----------------|
| **Windows** | x64, ARM64 | `.exe` installer |
| **macOS** | Intel (x64), Apple Silicon (ARM64) | `.dmg`, `.zip` |
| **Linux** | amd64, arm64 | `.deb`, `.AppImage` |

## Download

### Latest Version

Visit [GitHub Releases](https://github.com/tunnel6/yat/releases/latest) to download the latest version.

### All Versions

View [all releases](https://github.com/tunnel6/yat/releases) for historical versions and changelogs.

## Installation Guide

### Windows

1. Download the `.exe` installer
2. Double-click to run the installation wizard
3. Follow the prompts to complete installation

### macOS

1. Download the `.dmg` file
2. Open the DMG file
3. Drag YAT to the Applications folder

> **Note**: On first run, you may need to allow the app in "System Preferences > Security & Privacy"

### Linux (Debian/Ubuntu)

```bash
# Download .deb package
wget https://github.com/tunnel6/yat/releases/latest/download/yat_x.x.x_amd64.deb

# Install
sudo dpkg -i yat_x.x.x_amd64.deb

# If there are dependency issues, run
sudo apt-get install -f
```

### Linux (AppImage)

```bash
# Download AppImage
wget https://github.com/tunnel6/yat/releases/latest/download/yat_x.x.x_x86_64.AppImage

# Add execute permission
chmod +x yat_x.x.x_x86_64.AppImage

# Run
./yat_x.x.x_x86_64.AppImage
```

## Usage

### Quick Start

1. **Launch Application**: Start YAT from the application menu or command line
2. **Login**: Sign in with your YAT account (or create a new account)
3. **Create Tunnel**: Click "New Tunnel" button and configure your local service
4. **Access Service**: Use the generated public address to access your local service

### Main Features

- **HTTP/HTTPS Tunnels**: Expose local web services to the internet
- **TCP/UDP Tunnels**: Support for any TCP/UDP service penetration
- **WireGuard VPN**: Build secure virtual private networks
- **Remote Desktop**: ARD/VNC remote desktop support via extensions
- **Extension System**: Install custom extensions for enhanced functionality

## Updates

YAT client supports automatic updates:

- **macOS/Linux**: Automatically checks for updates on application startup
- **Windows**: Automatic updates through the installer

You can also manually check for updates: Help menu > Check for Updates

## Feedback & Support

- 📖 **Documentation**: [https://www.tunnel6.com/docs](https://www.tunnel6.com/docs)
- 🌐 **Official Website**: [https://www.tunnel6.com](https://www.tunnel6.com)
- 🐛 **Bug Reports**: [GitHub Issues](https://github.com/tunnel6/yat/issues)
- 💬 **Discussions**: [GitHub Discussions](https://github.com/tunnel6/yat/discussions)
- 📧 **Email**: support@tunnel6.com

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details

## Contributing

We welcome community contributions! If you'd like to participate in development, please visit the [main repository](https://github.com/tunnel6/yat) for more information.

## Related Links

- **Official Website**: [https://www.tunnel6.com](https://www.tunnel6.com)
- **Documentation**: [https://www.tunnel6.com/docs](https://www.tunnel6.com/docs)
- **Main Repository**: [https://github.com/tunnel6/yat](https://github.com/tunnel6/yat)
- **User Guide**: [https://www.tunnel6.com/docs/guide](https://www.tunnel6.com/docs/guide)
- **Download Page**: [https://www.tunnel6.com/download](https://www.tunnel6.com/download)
- **Extension Developer Guide**: [https://www.tunnel6.com/docs/developer](https://www.tunnel6.com/docs/developer)
