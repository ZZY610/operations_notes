# 连接 Connections

![](~@imgs/ui-connections.png)

在这个页面可以直观的看到当前的所有连接状态，可以一键关闭指定或全部连接

## 顶栏

- Pause/Resume: 暂停/继续连接统计
- Total: 显示当前上下行流量总量
- Search：搜索指定连接
- 排序按钮(依据):
  - Upload Speed 根据上行速度
  - Download Speed 根据下行速度
  - Upload Traffic 根据上行流量
  - Download Traffic 根据下行流量
  - Time 根据建立连接的时间
- Close All: 关闭全部连接

## 连接显示区域

- TCP/UDP: 连接协议
- HTTP Connect/HTTP/Socks5/TUN: 连接类型
- 禁止符号: 关闭连接

点击之后可查看更加详细的信息

- Alive/Closed: 连接是否存活
- Host: 连接到的主机
- Network: 连接协议
- Traffic: 此连接上/下行流量
- Source: 连接来源地址
- Destinaion: 连接目的地址（如 Host 已被 DNS 解析则显示对应 IP 地址）
- Rule: 连接命中的规则
- Process Path: 连接发起应用路径（需要经过 PROCESS-NAME 规则触发）
- Chains: 连接策略链
- Start Time: 建立连接的时间
