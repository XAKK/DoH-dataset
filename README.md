# DoH Traffic Dataset

Chinese (中文) README can be found [here](README_zh.md).

DNS-over-HTTPS is the most popular encrypted DNS protocol.

## 1 DoH Traffic Generation and Collection SystemDoH Traffic

Fig 1 DoH Traffic Generation and Collection System architecture

<img src="imgs/DTGCS.drawio.svg" style="zoom:25%;" />


## 2 Original DoH Traffic Dataset

Original DoH traffic dataset can be obtain by clicking [here](https://www.heywhale.com/mw/dataset/628b4994f498c246a27cfdf5/file).

The data set was generated from February 12, 2022 to February 25, 2022.

The domain name list of dataset contains 1000 domain names, originating from [Chinaz website](https://top.chinaz.com/alltop/](https://top.chinaz.com/alltop/)) ranking on February 6, 2022 (domain name file can be seen in `domains/chinaz-ranking-reachable-20220206.txt`). Domain names are selected in sequence. The websites that cannot provide services or do not use HTTPS are excluded in the domain name list.

In the dataset generation stage, the DNS-over-HTTPS server is AliDNS (IPv4 addresses are 223.5.5.5, 223.6.6.6). For each domain name, 100 DoH traffic sample were collected by visiting website repeatly.

The directory of final generated original DoH traffic dataset contains 1000 subdirectories, and the directory name of the subdirectory is the website domain name. Each subdirectory contains 100 files in pcap format. The size of the original dataset is 3.3 GB. More information about the dataset generation environment is shown in the table below.

Table 1 DoH traffic dataset generation environment

| Property                 | Value                                      |
| ------------------------ | ------------------------------------------ |
| CPU                      | Intel(R) Core(TM)2 Duo CPU T7700 @ 2.40GHz |
| CPU core number          | 12                                         |
| Memory                   | 24GB                                       |
| Downstream bandwidth     | >200Mbps                                   |
| Upstream bandwidth       | >200Mbps                                   |
| OS                       | Ubuntu 20.04.1 LTS amd64                   |
| Browser                  | Google Chrome (version 97.0.4692.99)       |
| Web automation framework | Selenium (version 4.1.0)                   |
| Traffic capture tool     | tcpdump (version 4.9.3)                    |
| DoH server               | AliDNS (https://dns.alidns.com/dns-query)  |

## 3 DoH Traffic Dataset

The DoH traffic dataset is obtained after cleaning and preprocessing the original DoH traffic dataset. The file format in the dataset is json, and the path is `dataset`.

A pcap file in the original DoH traffic dataset corresponds to a json file in the DoH traffic dataset, and a json file with some content omitted is as follows:

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

where,

* tuple `"(2886734945, 3741648133, 57244, 443, 'tcp', 0)"`  is the flow ID, the items in the tuple are source IP address, destination IP address, source port, destination port, protocol, and flow type (bidirectional or unidirectional) in turns;
* `"ts_list"` means the timestamp list of the packet sent, in seconds;
* `"ps_list"` means packet size list (bytes);
* `"direction_list"` indicates the direction of packets list. 0 indicates source-to-destination and 1 indicates destination to source;
* `"piat_list"` indicates the packet interarrival time (in ms) list, and the first value is 0.
