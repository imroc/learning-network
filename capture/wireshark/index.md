---
title: "wireshark 实用技巧"
type: book
date: "2021-07-20"
---

## 分析 DNS 异常

### 找出没有收到响应的 dns 请求

```txt
dns && (dns.flags.response == 0) && ! dns.response_in
```

### 根据 dns 请求 id 过滤

```txt
dns.id == 0xff0b
```

### 找出慢响应

超过 100 ms 的响应:

```txt
dns.flags.rcode eq 0 and dns.time gt .1
```

### 过滤 NXDomain 的响应

所有 `No such name` 的响应:

```txt
dns.flags.rcode == 3
```

排除集群内部 service:

```txt
((dns.flags.rcode == 3) && !(dns.qry.name contains ".local") && !(dns.qry.name contains ".svc") && !(dns.qry.name contains ".cluster"))
```

指定某个外部域名:

```txt
((dns.flags.rcode == 3) && (dns.qry.name == "imroc.cc")
```
