---
title: "tcpdump 实用技巧"
type: book
date: "2021-07-20"
---

## 抓包轮转
``` bash
# 每100M轮转一次，最多保留100个文件 (推荐)
tcpdump -i eth0 port 8880 -w cvm.pcap -C 100 -W 100

# 每2分钟轮转一次，后缀带上时间
tcpdump -i eth0 port 31780 -w node-10.70.10.101-%Y-%m%d-%H%M-%S.pcap -G 120
```

