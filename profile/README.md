# Flex Access Dev

Open-source tunneling and VPN utilities for reaching private networks, with native clients for Apple platforms, Windows, and iOS.

Everything here shares the same core idea: the client dials the server by a stable cryptographic identity, with NAT traversal and relay fallback, so the **server needs no public IP, no open inbound port, and no port forwarding** — and all traffic is end-to-end encrypted.

## Projects

### ezvpn — easy-setup VPN

A full VPN: creates a virtual network interface and routes IP packets (IPv4/IPv6, split or full tunnel) through encrypted connections. Dynamic client IP assignment means no VPN subnet planning. Designed for easy, temporary split-tunnel access to a server's network — e.g. reaching private cloud resources — without the classic VPN setup friction.

| Repo | What it is |
|---|---|
| [ezvpn](https://github.com/flexaccessdev/ezvpn) | The Rust core: cross-platform IP-over-QUIC VPN with NAT traversal via iroh (CLI client + server) |
| [ezvpn-apple](https://github.com/flexaccessdev/ezvpn-apple) | Universal native iOS + macOS SwiftUI app with a Packet Tunnel extension driving the Rust core |
| [ezvpn-windows](https://github.com/flexaccessdev/ezvpn-windows) | Native Windows GUI (WinUI 3, .NET) driving the Rust core via FFI, using wintun |

### flextunnel — rootless proxy split tunnel

Proxy-level access without a VPN: the client runs local SOCKS5/HTTP proxy listeners, and routed targets are tunneled over an encrypted connection to the server, which resolves DNS and makes the outbound TCP connection from its own network. Uses ordinary userspace sockets — no virtual network interface, so **neither end needs root/admin**.

| Repo | What it is |
|---|---|
| [flextunnel](https://github.com/flexaccessdev/flextunnel) | The Rust client + server: SOCKS5/HTTP-proxy-over-QUIC split tunnel |
| [flextunnel-ios](https://github.com/flexaccessdev/flextunnel-ios) | iOS client: browse private networks and forward local ports through the tunnel — split-tunnel by default, no VPN required |

## Which one do I want?

- **ezvpn** — you need real IP routing to a private network (whole subnets, any protocol). Requires root/Administrator to create the virtual network interface.
- **flextunnel** — you need TCP access to hosts behind the server (web UIs, SSH, databases) and want zero-privilege setup on both ends, or a per-app proxy instead of system-wide routing. Because it isn't a VPN, it also sidesteps iOS's one-active-VPN-at-a-time restriction — it can run alongside another VPN on the device.

## Notes

- These tools provide **access to networks you control** — both ends are run by the same trusted party, with token-based client authentication. They are not anonymity networks or multi-tenant services.
- Never commit credentials, auth tokens, private keys, or connection profiles. Follow each repository's setup and security-model documentation.
