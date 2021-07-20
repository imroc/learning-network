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

## 过滤连接超时的包(reset)

``` bash
tcpdump -r test.pcap 'tcp[tcpflags] & (tcp-rst) != 0' -nn -ttt
```

## 统计流量源ip

``` bash
tcpdump -i eth0 dst port 60002 -c 10000|awk '{print $3}'|awk -F. -v OFS="." '{print $1,$2,$3,$4}'|sort |uniq -c|sort -k1 -n
```

统计效果：
```
    321 169.254.128.100
    409 10.0.0.175
   2202 10.0.226.49
```


## 读取 tcpdump 抓包文件

``` bash
tcpdump -nn -tttt -r <file>
```

* -r 指定包文件
* -nn 显示数字ip和端口，不转换成名字
* -tttt 显示时间戳格式: 2006-01-02 15:04:05.999999