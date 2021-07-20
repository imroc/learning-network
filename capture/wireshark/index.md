---
title: "wireshark 实用技巧"
type: book
date: "2021-07-20"
---

## 分析 DNS 异常

### 没有收到响应的 dns 请求

```txt
dns && (dns.flags.response == 0) && ! dns.response_in
```

### 过滤 NXDomain 的响应

所有 `No such name` 的响应:

```txt
((dns.flags.rcode == 3) && !(dns.qry.name contains ".local") && !(dns.qry.name contains ".svc") && !(dns.qry.name contains ".cluster"))
```

所有外部域名:

```txt
((dns.flags.rcode == 3) && !(dns.qry.name contains ".local") && !(dns.qry.name contains ".svc") && !(dns.qry.name contains ".cluster"))
```

指定外部域名:

```txt
((dns.flags.rcode == 3) && (dns.qry.name == "imroc.cc")
```