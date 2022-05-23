# DoH 流量数据集

DNS-over-HTTPS 协议是当前最主流的加密 DNS 协议。

## 1 DoH 流量生成与收集系统

<img src="imgs/DoH流量生成与收集系统.drawio.svg" style="zoom:80%;" />

图1 DoH 流量生成与收集系统架构

## 2 原始 DoH 流量数据集

原始的 DoH 数据集可从[这里](https://www.heywhale.com/mw/dataset/628b4994f498c246a27cfdf5/file)获取。

数据集的生成时间为 2022 年 2 月 12 日至 2022 年 2 月 25 日。

数据集域名列表包含了 1000 个网站域名，域名都来源于 [Chinaz 网站](https://top.chinaz.com/alltop/) 2022 年 2 月 6 日统计的网站排名（域名文件见 `domains/chinaz-ranking-reachable-20220206.txt`）。域名按照排名的先后，依次选择。对于无法正常提供服务，或未使用 HTTPS 协议的网站，其对应的域名不纳入域名列表的选择范围之内。

在数据集生成阶段，使用的 DNS-over-HTTPS 服务端为 AliDNS（IPv4 地址为 223.5.5.5、223.6.6.6）。对每个域名以循环访问的方式，收集 100 条 DoH 流量样本。

最终生成的原始数据集，目录下包含 1000 个子目录，子目录的目录名为网站域名。每个子目录下包含 100 个 pcap 格式的文件，都为访问子目录名对应的网站域名所产生的 DoH 流量。原始数据集的大小为 3.3 GB。更多与该数据集生成环境相关的信息如下表所示。

表1 DoH流量数据集生成环境

| 属性          | 值                                         |
| ------------- | ------------------------------------------ |
| CPU型号       | Intel(R) Core(TM)2 Duo CPU T7700 @ 2.40GHz |
| CPU核数       | 12                                         |
| 内存大小      | 24 GB                                      |
| 下行最大带宽  | > 200 Mbps                                 |
| 上行最大带宽  | > 200 Mbps                                 |
| 操作系统      | Ubuntu 20.04.1 LTS amd64                   |
| 浏览器        | Google Chrome (version 97.0.4692.99)       |
| Web自动化框架 | Selenium (version 4.1.0)                   |
| 流量抓取工具  | tcpdump (version 4.9.3)                    |
| DoH服务端     | AliDNS (https://dns.alidns.com/dns-query)  |

## 3 DoH 流量数据集

DoH 流量数据集是将原始 DoH 流量数据集进行清洗和预处理后获得的数据集，数据集中的文件格式为 json，所在路径为 `dataset`。

一个原始 DoH 流量数据集中的 pcap 文件，对应 DoH 流量数据集中的一个 json 文件，一个省略了部分内容的结果 json 文件如下所示：

```
{
    "(2886734945, 3741648133, 57244, 443, 'tcp', 0)": {
        "ts_list": [...],
        "ps_list": [...],
        "direction_list": [...],
        "piat_list": [...]
    }
}
```

其中，

* 元组 `"(2886734945, 3741648133, 57244, 443, 'tcp', 0)"` 表示流的唯一标识符（流 ID），元组中的每一项分别表示源 IP、宿 IP、源端口、宿端口、协议和流类型（双向流还是单向流）；
* `"ts_list"` （timestamp）表示数据包发送的时间戳，单位为秒；
* `"ps_list"` （packet size）表示数据包的大小，单位为字节；
* `"direction_list"` 表示数据包方向，0 为源到宿，1 为宿到源；
* `"piat_list"` （packet interarrival time）数据包较前一个数据的的间隔时长，单位为毫秒，第一个值为 0；
