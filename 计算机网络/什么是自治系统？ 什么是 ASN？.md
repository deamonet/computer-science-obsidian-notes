自治系统（AS）是具有单个路由策略的巨型网络或网络群组。每个 AS 分配一个唯一的 ASN，ASN 是标识 AS 的编号。

## 什么是自治系统？

[Internet](https://www.cloudflare.com/learning/network-layer/how-does-the-internet-work/) 是由不同网络组成的网络*，自治系统是组成 Internet 的大型网络。更具体地说，自治系统（AS）是具有统一[路由](https://www.cloudflare.com/learning/network-layer/what-is-routing/)策略的巨型网络或网络群组。连接到 Internet 的每台计算机或设备都连接到一个 AS。

![具有 ASN 的全球自治系统](https://cf-assets.www.cloudflare.com/slt3lc6tev37/2VQ6NpacA6xXz9B8iAE7re/e06c5e47d5138d05b27c208a59373a30/autonomous-system-diagram.svg)

可以认为 AS 类似于一个城镇的邮局。邮件从一个邮局到另一个邮局，直到到达正确的城镇为止，然后该城镇的邮局将在该城镇内传递邮件。与之类似，[数据包](https://www.cloudflare.com/learning/network-layer/what-is-a-packet/)在整个互联网范围内通过从 AS 跳到 AS，直到它们到达包含其目的地[互联网协议 (IP)](https://www.cloudflare.com/learning/ddos/glossary/internet-protocol/) 地址的 AS。该 AS 中的[路由器](https://www.cloudflare.com/learning/network-layer/what-is-a-router/)将数据包发送到 [IP 地址](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/)。

每个 AS 都控制一组特定的 IP 地址，就像每个镇的邮局负责将邮件传递到该镇内的所有地址一样。给定 AS 可以控制的 IP 地址范围称为其“IP 地址空间”。

大多数 AS 连接到其他几个 AS。如果一个 AS 仅连接到另一个 AS 并共享相同的路由策略，则可以将其视为第一个 AS 的子网。

通常，每个 AS 由单个大型组织（例如 Internet 服务提供商（ISP）、大型企业技术公司、大学或政府机构）运营。

_*网络是由两台或更多台连接的计算机组成的群组。_

## 什么是 AS 路由策略？

AS 路由策略是由 AS 控制的 IP 地址空间的列表以及与其连接的其他 AS 的列表。此信息对于将数据包路由到正确的网络是必需的。AS 使用[边界网关协议（BGP）](https://www.cloudflare.com/learning/security/glossary/what-is-bgp/)向 Internet 通知此信息。

## 什么是 IP 地址空间？

IP 地址的指定群组或范围称为“IP 地址空间”。每个 AS 控制一定数量的 IP 地址空间。（一组 IP 地址也可以称为 IP 地址“块”。）

这种情况类似于如果世界上的所有电话号码都按顺序列出，并且为每个电话公司分配一个范围：电话公司A 控制号码 000-0000 至 599-9999，电话公司 B 控制号码 600-0000 至 999-9999。如果爱丽丝拨打 555-2424 致电米歇尔，她的致电将通过电话公司 A 路由到米歇尔。如果她拨打 867-5309 致电珍妮，她的致电将通过电话公司 B 路由到珍妮。

这就是 IP 地址空间的工作方式。假设 Acme 公司运营一家 AS，并控制一个 IP 地址范围，其中包括地址 192.0.2.253。如果计算机将数据包发送到 192.0.2.253，则该数据包最终将到达由 Acme 公司控制的 AS。如果该第一台计算机也将数据包发送到 198.51.100.255，则数据包将到达另一个 AS（尽管它们可能会在途中通过 Acme 公司的 AS）。

## 什么是 IP 地址前缀？

当网络工程师通知哪个 IP 地址由哪个 AS 控制时，他们通过提及每个 AS 拥有的 IP 地址“前缀”来实现。IP 地址前缀是 IP 地址的范围。根据 IP 地址的写入方式，IP 地址前缀通过以下方式表示：192.0.2.0/24。这表示 IP 地址 192.0.2.0 到 192.0.2.255，而不是 192.0.2.0 到 192.0.2.24。

## 什么是自治系统编号（ASN）？

每个 AS 都分配有一个官方编号或自治系统编号（ASN），类似于每个企业如何获得具有唯一官方编号的营业执照。但是，与企业不同，外部各方通常仅通过编号来引用 AS。

AS 编号或 ASN 是介于 1 和 65534 之间的唯一 16 位数字或介于 131072 和 4294967294 之间的 32 位数字。它们通过这种格式表示：AS（数字）。例如，Cloudflare 的 ASN 是 AS13335。据不完全估计，全世界有超过 90,000 个 ASN 在使用。

只有与网络间路由器的外部通信才需要 ASN（请参阅下面的“什么是 BGP？”）。AS 中的内部路由器和计算机可能不需要知道该 AS 的编号，因为它们仅与该 AS 内的设备通信。

在分配 ASN 的管理机构给它编号之前，AS 必须满足某些资格。它必须具有唯一的路由策略，具有一定的大小，并且与其他 AS 具有多个连接。可用的 ASN 数量有限，如果过度分发它们，则供应会用光，路由也会变得更加复杂。

## 什么是 BGP？

AS 通过边界网关协议（BGP）通知其到其他 AS 和路由器的路由策略。BGP 是在 AS 之间路由数据包的协议。如果没有这些路由信息，大规模运行 Internet 将很快变得不切实际：数据包将丢失或花费太长时间才能到达目的地。

每个 AS 使用 BGP 通知它们负责的 IP 地址以及它们连接的其他 AS。BGP 路由器从世界各地的 AS 中获取所有这些信息，并将其放入称为路由表的数据库中，以确定从 AS 到 AS 的最快路径。当数据包到达时，BGP 路由器会参考其路由表来确定数据包接下来应转到哪个 AS。

全世界有许多 AS，BGP 路由器不断更新其路由表。网络脱机时，新网络联机时，AS 扩展或收缩其 IP 地址空间，所有这些信息都必须通过 BGP 通知，以便 BGP 路由器可以调整其路由表。

## 为什么需要 BGP 路由？IP 不用于路由吗？

IP（Internet 协议）确实用于路由，因为它指定每个数据包将到达的目的地。BGP 负责将数据包以最快的路由定向到目的地。如果没有 BGP，IP 数据包将在 Internet 上从 AS 到 AS 随机反弹，类似于驾驶员试图通过猜测走哪条路来到达目的地。

## 自治系统如何相互连接？

AS 之间相互连接，并通过称为对等的过程交换网络流量（数据包）。AS 彼此对等的一种方式是通过在称为 [Internet 交换点（IXP）的](https://www.cloudflare.com/learning/cdn/glossary/internet-exchange-point-ixp/)物理位置进行连接。IXP 是一个大型[局域网（LAN），](https://www.cloudflare.com/learning/network-layer/what-is-a-lan/)具有许多路由器、交换机和电缆连接。