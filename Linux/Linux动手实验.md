plantegg

https://twitter.com/plantegg/status/1639633374901080064?s=46&t=NWSzHW1s4CGWjKDTJSJ9Vg

在星球里留了一个一年必须要动手做的实验，主要目的是做完对后面的案例、分析、debug手段比较熟悉，后面不用再展开讲了；其次的目的是想让大家体会一下做会和看会的区别。你们猜给一个月时间能有多少人会真正参与做完？即使交了钱我估计参与做完的也不会超过50%，大部分会被操作卡点劝退了，你可以试试


程序员经典案例分享
2023-03-24 07:00
实验动起来
Linux to 工具控制网络时延、丟包率，实验神器
https://plantegg.github.io/2016/08/24/Linux%20tc%2..
实验：内网搞两台Linux，其中一台启动服务(当前目录放一个2G的大文件）：python -m SimpleHT TPServer
8089
另外一台机器通过 curl 访问8089下载这个文件，tcpdump抓包，wireshark分析
按照：
着RT增加慢下来
TCP性能和发送接收窗口、Buffer的关系 1 plantegg 实验调整RT、控制死内核tcp buffer，让速度随
写死Linux参数让传输速度慢下来(sysctl-w 搞错了就重启机器)：
net.ipv4.tcp_rmem=4096
4096
4096 //最小值 默认值 最大值
继续调整r、丢包率，curl限制速度等，抓包分析看到的所有现象
试验完成后:
tc 控制网络熟悉得一逼
tcpdump、wireshark入门不怵了
tcp传输速度分析、BDP..
凡是不清楚就Google，读curl help如何控制速度，养成他妈的读帮助的习惯
这这一年我只要求一定要把这个实验全程搞完，就能达到我上述目的，特意发在周五，大家周末开搞