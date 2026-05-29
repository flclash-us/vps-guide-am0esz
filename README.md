# OpenWrt Proxy Setup 9bv60j

Complete guide to setting up proxy services on OpenWrt routers with OpenClash, including transparent proxy, DNS resolution, and routing rules.

## Supported Routers

| Router | CPU | RAM | Flash | Recommended |
|--------|-----|-----|-------|-------------|
| Xiaomi AX3000T | MT7981B | 256MB | 128MB | Yes |
| GL-iNet MT-3000 | MT7981B | 512MB | 128MB | Best |
| Redmi AX6000 | MT7986A | 512MB | 128MB | Yes |
| Raspberry Pi 4B | BCM2711 | 4GB | SD | Good |

## Installation

### 1. Install OpenClash

```bash
# Add OpenClash feed
echo "src-git openclash https://github.com/vernesong/OpenClash.git" >> feeds.conf.default

# Update feeds
./scripts/feeds update openclash
./scripts/feeds install -a -p openclash

# Or install via LuCI (recommended)
# System -> Software -> Upload ipk
```

### 2. Core Installation

```bash
# Download Clash Premium core
# In LuCI: Services -> OpenClash -> Version Update
# Select "Clash Premium" and click Update
```

### 3. Basic Configuration

```bash
# Import subscription
# Services -> OpenClash -> Config Subscribe
# Add your subscription URL
# Click "Update Configuration"
```

## Transparent Proxy Setup

### DNS Settings
```bash
# In OpenClash settings:
# Operation Mode: Redir-Host (compatible) or TUN (performance)
# DNS Mode: Fake-IP (recommended)
# Fake-IP Range: 198.18.0.1/16
```

### Firewall Rules
```bash
# OpenClash auto-configures iptables/nftables rules
# Verify with:
iptables -t nat -L -n | grep -i clash
```

## Performance Tuning

```bash
# Increase UDP buffer for Hysteria2
echo "net.core.rmem_max=16777216" >> /etc/sysctl.conf
echo "net.core.wmem_max=16777216" >> /etc/sysctl.conf
sysctl -p

# Enable hardware offload
ethtool -K eth0 gro on gso on tso on
```

## Troubleshooting

| Issue | Solution |
|-------|----------|
| No internet | Check DNS settings, try Redir-Host mode |
| Slow speed | Disable rule-provider, use simple rules |
| Memory low | Use Clash Meta core, reduce fake-ip range |
| DHCP conflict | Disable dnsmasq DNS when using OpenClash |

## Recommended Tools

- [Clash for Windows](https://clashforwindows.site/) - Best proxy client for Windows/Mac/Linux
- [Clash for Windows](https://clashforwindows.site/) - Most popular Clash GUI
- [ClashMI](https://clashmi.site/) - Lightweight Clash client
- [FlClash](https://flclash.us/) - Modern proxy tool

## License

MIT License
