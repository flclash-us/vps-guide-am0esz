# Clash Fake IP 模式详解

Fake IP 是 Clash 最重要的 DNS 工作模式，本文详细介绍原理、配置与最佳实践。

## 什么是 Fake IP

Fake IP（伪造 IP）模式工作原理：

```
1. 客户端请求 github.com
2. Clash 拦截并返回假 IP (198.18.x.x)
3. 客户端连接假 IP
4. Clash 拦截连接，查表还原真实域名
5. 通过代理转发请求
```

## 为什么需要 Fake IP

| 传统 DNS 方式 | Fake IP 方式 |
|--------------|-------------|
| DNS 泄露真实域名 | 无域名泄露 |
| 需要 DNS 代理 | 可直连 DNS |
| 配置复杂 | 配置简单 |

## 基本配置

```yaml
dns:
  enable: true
  listen: 0.0.0.0:53
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  
  nameserver:
    - https://doh.pub/dns-query
    - https://dns.alidns.com/dns-query
  
  fallback:
    - https://1.1.1.1/dns-query
  
  fallback-filter:
    geoip: true
    geoip-code: CN
```

## Fake IP Filter

排除不需要代理的域名：

```yaml
dns:
  enhanced-mode: fake-ip
  fake-ip-filter:
    - "*.lan"
    - "*.local"
    - "*.baidu.com"
    - "*.taobao.com"
    - "*.alipay.com"
```

## DNS 泄露测试

访问 https://dnsleaktest.com/ 检测是否存在 DNS 泄露。

## 常见问题

**部分网站打不开？** 添加域名到 `fake-ip-filter`。

---

推荐工具：

- [Clash for Windows](https://clashforwindows.site/)
- [ClashMI](https://clashmi.site/)
- [FlClash](https://flclash.us/)
