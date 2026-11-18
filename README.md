# OpenYAT

[![License: Apache-2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0)

[English](README.md) | [中文](README_ZH.md)

**Official website**: <https://tunnel6.com>

OpenYAT is the open-source client and private networking solution for YAT (Yet Another Tunnel). It uses a self-hosted **Edge** as the runtime coordination node, authenticates devices with mTLS, and provides intranet tunneling, TCP/UDP/HTTP forwarding, and private WireGuard networking.

The OpenYAT runtime does not require a platform account. Devices generate their identity and key material locally, then use a one-time pairing token or network invitation to request a device certificate from Edge. Subsequent communication is verified by the Edge self-signed CA and encrypted with mTLS.

## Key Capabilities

- **Private deployment**: Edge can be deployed on your own VPS, cloud server, or any publicly reachable server, keeping runtime data and state under your control.
- **Zero-trust connectivity**: Clients and Edge use mutual certificate authentication. Device identity, Owner/Device permissions, and network authorization are constrained by the certificate chain.
- **Intranet tunneling**: Expose HTTP/HTTPS, TCP, and UDP services to the public network. It is suitable for NAS devices, development environments, intranet databases, IoT devices, and similar scenarios.
- **WireGuard networking**: Build virtual device networks with the local WireGuard adapter. Supported path strategies include direct-first, Edge relay fallback, and forced relay.
- **Multiple clients**: The desktop client targets macOS, Windows, and Linux. `yat-fe-linux` provides a CLI/daemon experience for Linux servers, embedded systems, and automation scenarios.
- **Extensible runtime**: Clients and Edge synchronize tunnel/channel state through a protocol-based mechanism. Edge coordinates business state and converges it across subscribed devices.

## Architecture Overview

OpenYAT separates the business layer, protocol transport layer, and local network execution layer into independent projects. The core dependency relationships are:

```text
                 ┌───────────────┐
                 │     Edge      │
                 │ Edge runtime  │
                 └──┬─────────▲──┘
                    │         │
                mTLS / protocol sync
                    │         │
        ┌───────────▼─────────┴───────────┐
        │ OpenYAT desktop / yat-fe-linux  │
        └───────────────┬─────────────────┘
                        │
                        ▼
                  ┌───────────┐
                  │  network  │
                  │ Client core│
                  └────┬──┬───┘
                       │  │
          ┌────────────┘  └────────────┐
          ▼                           ▼
   ┌────────────┐              ┌────────────┐
   │ transport  │              │  wireguard │
   └────────────┘              └────────────┘
```

In concise form:

```text
Edge <──> OpenYAT / yat-fe-linux
OpenYAT / yat-fe-linux -> network -> { transport | wireguard }
```

### Runtime Responsibilities

| Layer | Project | Responsibilities |
| --- | --- | --- |
| Server | [Edge](https://github.com/tunnel6/edge.git) | Edge runtime and tunnel coordinator; maintains device sessions, certificate issuance, channel state, network metadata, WireGuard relay, and client synchronization events. |
| Client business layer | [network](https://github.com/tunnel6/network.git) | Core client business processing; handles pairing, certificate requests, network/tunnel state aggregation, local runtime configuration, and Electron/CLI call bridging. |
| Protocol transport layer | [transport](https://github.com/tunnel6/transport.git) | Protocol transport between Edge and clients; carries mTLS sessions, FlatBuffers messages, tunnel control, and state synchronization. |
| Local WireGuard execution layer | [wireguard](https://github.com/tunnel6/wireguard.git) | WireGuard protocol wrapper and WG business control layer; manages local keys, peers, AllowedIPs, interfaces, routes, and runtime state. |
| Desktop client | OpenYAT | Electron + Vue desktop client; provides management UI for networks, tunnels, Edge, pairing, and runtime status. |
| Linux CLI | [yat-fe-linux](https://github.com/tunnel6/yat-fe-linux.git) | Linux client CLI edition; suitable for servers, headless environments, systemd services, and automated operations. |

## Related Projects

| Project | Address | Description |
| --- | --- | --- |
| Edge | <https://github.com/tunnel6/edge.git> | Edge service; the runtime coordinator and public access node for OpenYAT. |
| network | <https://github.com/tunnel6/network.git> | Core client business processing shared by the desktop client and Linux CLI. |
| transport | <https://github.com/tunnel6/transport.git> | Protocol transport implementation between Edge and clients. |
| wireguard | <https://github.com/tunnel6/wireguard.git> | WireGuard protocol wrapper and WG business control layer. |
| yat-fe-linux | <https://github.com/tunnel6/yat-fe-linux.git> | Linux client CLI edition. |

## Runtime Flow

1. **Deploy Edge**: Start Edge on a publicly reachable server. On first startup, Edge generates its own CA and service certificate.
2. **Pair devices**: An Edge administrator generates a one-time Owner or Device pairing token. The client uses that token to connect to Edge and request a device certificate.
3. **Establish an mTLS session**: After the client stores its device identity, subsequent tunnel control, channel management, and state synchronization use an mTLS session.
4. **Create or join a network**: An Owner creates a WireGuard network, and other devices join through an invitation package. The shared package contains only invitation credentials and the public CA; it does not export the client private key.
5. **Synchronize business state**: `network` synchronizes network, tunnel, and channel state with Edge through `transport`; `wireguard` applies network configuration to the local interface, routes, and peer runtime.
6. **Select the data path**: Device-to-device communication first attempts a direct connection. If direct connectivity is unavailable, it can switch to Edge relay according to the network configuration.

## Installation

### Desktop

Download the package for your platform from [GitHub Releases](https://github.com/tunnel6/yat/releases).

| Platform | Architecture | Package |
| --- | --- | --- |
| Windows | x64 / ARM64 | `.exe` |
| macOS | x64 / ARM64 | `.dmg`, `.zip` |
| Linux | amd64 / arm64 | `.deb`, `.AppImage` |

#### Windows

1. Download the `.exe` installer.
2. Double-click it and follow the installation wizard.

#### macOS

1. Download and open the `.dmg` file.
2. Drag OpenYAT into the Applications folder.
3. If Gatekeeper blocks the first launch, allow the app in **System Settings > Privacy & Security**.

#### Linux

```bash
# Debian / Ubuntu
sudo dpkg -i openyat_<version>_amd64.deb

# Fix dependency issues, if any
sudo apt-get install -f
```

```bash
# AppImage
chmod +x openyat_<version>_x86_64.AppImage
./openyat_<version>_x86_64.AppImage
```

> Replace `<version>` in the filenames with the actual downloaded version.

### Linux CLI

For servers or headless environments, use [yat-fe-linux](https://github.com/tunnel6/yat-fe-linux). The common workflow is:

```bash
# Pair with Edge
yat-client pair --edge example.com:9443 --token <PAIR_TOKEN>

# Import a network invitation package
yat-client import <INVITE_FILE>
```

See the README and CLI help in the `yat-fe-linux` repository for exact command options.

## Quick Start

1. Start the OpenYAT desktop client or `yat-client`.
2. Enter the Edge address and one-time pairing token to pair the device.
3. Create a WireGuard network on an Owner device.
4. Export the network invitation and import it on another device.
5. Check the network members page to confirm that devices are online and have assigned IP addresses and connection paths.
6. Create HTTP/HTTPS, TCP, or UDP tunnels as needed, and access your intranet services through the addresses returned by Edge.

## Security Model

- **Device identity**: Clients generate device keys locally and do not depend on platform account passwords.
- **Autonomous Edge CA**: Each Edge has its own certificate trust root. Clients store the corresponding CA for each Edge.
- **One-time authorization**: Pairing tokens and network invitations are consumed once, avoiding long-lived shared secrets.
- **Permission isolation**: Owner, Device, and invitation scopes are constrained by certificate extensions and the Edge session. Client requests cannot forge an identity or expand authorization.
- **Minimal sharing**: Network shares contain only the invitation and public CA; client certificate private keys are never exported.
- **Transport encryption**: Business control and state synchronization use mTLS, while the WireGuard data plane uses its native encryption protocol.

## Documentation

- YAT guide: <https://tunnel6.com/guide/what-is-yat.html>
- Edge deployment and configuration: <[https://tunnel6.com/guide//guide/edge/deploy.html](https://tunnel6.com/guide/edge/deploy.html)>
- WireGuard networking: <[https://tunnel6.com/guide/guide/wireguard-networking.html](https://tunnel6.com/guide/wireguard-networking.html)>
- Transport modes: <https://tunnel6.com/guide/guide/transport-modes.html>
- GitHub Issues: <https://github.com/tunnel6/yat/issues>

## Contributing

You are welcome to report issues or submit pull requests to the projects listed above. Before contributing, read the build and test instructions in the relevant repository and make sure Edge, network, transport, wireguard, and client binaries are not built from incompatible versions.

## License

The OpenYAT client is licensed under Apache License 2.0. Each related project is governed by the license in its own repository.
