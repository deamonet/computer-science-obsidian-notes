[[WSL 2] Multi WSL2 distributions use the same network namespace · Issue #4304 · microsoft/WSL (github.com)](https://github.com/microsoft/WSL/issues/4304)

# [WSL 2] Multi WSL2 distributions use the same network namespace #4304

# Issue

Multi WSL2 distributions use the same network namespace, no network isolation

# Issue Details

-   Your Windows build number: Microsoft Windows [版本 10.0.18936.1000]
    
-   What you're doing and what's happening:
    

1.  wsl --list -v  
    NAME STATE VERSION  
    Ubuntu-16.04 Running 2  
    Ubuntu-16.04-b Running 2  
    centos Running 2
    
2.  In Ubuntu-16.04 / Ubuntu-16.04-b / centos, the eth0 has the same IP  
    root@DESKTOP-ASI6ES4:~$ ifconfig eth0 | grep "inet addr"  
    inet addr: _172.30.114.66_ Bcast:172.30.255.255 Mask:255.255.0.0
    
    It seems that all the distributions share the same network namespace  
    Start a webserver on two distributions:  
    `python3 -m http.server 8000`,  
    the second distribution will get failed:
    

`root@DESKTOP-ASI6ES4:/chesstop/log# python3 -m http.server 8000 Traceback (most recent call last): File "/usr/lib/python3.5/runpy.py", line 184, in _run_module_as_main "__main__", mod_spec) File "/usr/lib/python3.5/runpy.py", line 85, in _run_code exec(code, run_globals) File "/usr/lib/python3.5/http/server.py", line 1221, in <module> test(HandlerClass=handler_class, port=args.port, bind=args.bind) File "/usr/lib/python3.5/http/server.py", line 1194, in test httpd = ServerClass(server_address, HandlerClass) File "/usr/lib/python3.5/socketserver.py", line 440, in __init__ self.server_bind() File "/usr/lib/python3.5/http/server.py", line 138, in server_bind socketserver.TCPServer.server_bind(self) File "/usr/lib/python3.5/socketserver.py", line 454, in server_bind self.socket.bind(self.server_address) OSError: [Errno 98] Address already in use`

-   What's wrong / what should be happening instead:  
    Each distribution should have its own network namespace

👍3avinoamsn, vbrozik, and np-8 reacted with thumbs up emoji

[![@craigloewen-msft](media/@craigloewen-msft-1.jpg)](https://github.com/craigloewen-msft) [craigloewen-msft](https://github.com/craigloewen-msft) added the [bydesign](https://github.com/microsoft/WSL/labels/bydesign) label [on Jul 16, 2019](https://github.com/microsoft/WSL/issues/4304#event-2483634807)

[![@craigloewen-msft](media/@craigloewen-msft.jpg)](https://github.com/craigloewen-msft)

Member

### 

**[craigloewen-msft](https://github.com/craigloewen-msft)** commented [on Jul 16, 2019](https://github.com/microsoft/WSL/issues/4304#issuecomment-511494636)

This is done by design, and so I'll mark this issue as closed, but am more than happy to follow up on a discussion about it.

What scenario are you trying to enable by having the distributions have different networking namespaces?

[![@craigloewen-msft](media/@craigloewen-msft-1.jpg)](https://github.com/craigloewen-msft) [craigloewen-msft](https://github.com/craigloewen-msft) closed this as [completed](https://github.com/microsoft/WSL/issues?q=is%3Aissue+is%3Aclosed+archived%3Afalse+reason%3Acompleted) [on Jul 16, 2019](https://github.com/microsoft/WSL/issues/4304#event-2483666435)

[![@prabhah](media/@prabhah.jpg)](https://github.com/prabhah)

Author

### 

**[prabhah](https://github.com/prabhah)** commented [on Jul 16, 2019](https://github.com/microsoft/WSL/issues/4304#issuecomment-511629954)

> This is done by design, and so I'll mark this issue as closed, but am more than happy to follow up on a discussion about it.
> 
> What scenario are you trying to enable by having the distributions have different networking namespaces?

Thanks for your response. I think WSL/WSL2 is a great feature for microsoft product. I'd like to try it in our work environment.  
We develop cluster softwares which works on more that one node, we use virtualbox / vmware ... to setup our development environment before, but these virtual technologies are too heavy for development machine. if WLS2 support network isolation, we can use this lightweight virtual techonlogy to achieve our purpose.

👍2Dzemy and fenjen reacted with thumbs up emoji

[![@craigloewen-msft](media/@craigloewen-msft.jpg)](https://github.com/craigloewen-msft)

Member

### 

**[craigloewen-msft](https://github.com/craigloewen-msft)** commented [on Jul 16, 2019](https://github.com/microsoft/WSL/issues/4304#issuecomment-511643531)

You can create new processes with new network namespaces, so yes you can achieve network isolation.

[![@prabhah](media/@prabhah.jpg)](https://github.com/prabhah)

Author

### 

**[prabhah](https://github.com/prabhah)** commented [on Jul 16, 2019](https://github.com/microsoft/WSL/issues/4304#issuecomment-511647699) • edited 

> You can create new processes with new network namespaces, so yes you can achieve network isolation.

thanks for reminding me,

`  
ip netns add net1

ip netns exec net1 ip addr add 192.168.99.10/24 dev sit0

ip netns exec net1 exec bash  
...  
`  
you mean this?  
Great Idea :) but it's indirectly way

👍3avinoamsn, rodrigobrito, and vbrozik reacted with thumbs up emoji

[![@craigloewen-msft](media/@craigloewen-msft.jpg)](https://github.com/craigloewen-msft)

Member

### 

**[craigloewen-msft](https://github.com/craigloewen-msft)** commented [on Jul 16, 2019](https://github.com/microsoft/WSL/issues/4304#issuecomment-511658009)

Yes that's exactly correct!

And to check my understanding, you're asking that you'd prefer each distro was in its own networking namespace rather than creating your own network namespaces, because it would be a more direct way? Or easier to setup? I'm sorry as I'm still confused on the feature request, or what you're asking.

👍2relief-melone and rodrigobrito reacted with thumbs up emoji

[![@prabhah](media/@prabhah.jpg)](https://github.com/prabhah)

Author

### 

**[prabhah](https://github.com/prabhah)** commented [on Jul 16, 2019](https://github.com/microsoft/WSL/issues/4304#issuecomment-511661499)

English is not my native language, sorry for my imprecision description to make you confuse.  
At the beginning of the problem，after I created multi WSL2 instances/distros, I found they used the same IP, I guess that they share the same network namespace.  
I just want to create a multi-nodes development environment, eath instance/distro have their own IP.  
Thanks for your help again.

[![@craigloewen-msft](media/@craigloewen-msft.jpg)](https://github.com/craigloewen-msft)

Member

### 

**[craigloewen-msft](https://github.com/craigloewen-msft)** commented [on Jul 17, 2019](https://github.com/microsoft/WSL/issues/4304#issuecomment-511884889)

No problem at all! Thank you for clarifying.

All of the WSL 2 distros run on the same virtual machine, which has a singular virtualized networking interface controller. You can [create different IP addresses](https://www.ostechnix.com/how-to-assign-multiple-ip-addresses-to-single-network-card-in-linux/) and different networking namespaces just like you would on a Linux machine to create network isolation for multi-node development.

👍2gt-novelt and np-8 reacted with thumbs up emoji

[![@cerebrate](media/@cerebrate.jpg)](https://github.com/cerebrate) [cerebrate](https://github.com/cerebrate) mentioned this issue [on Jul 28, 2019](https://github.com/microsoft/WSL/issues/4304#ref-issue-473713037)

[Questions: implementation of WSL 2 networking #4346](https://github.com/microsoft/WSL/issues/4346)

 Closed

[![@cpbotha](media/@cpbotha.jpg)](https://github.com/cpbotha)

### 

**[cpbotha](https://github.com/cpbotha)** commented [on May 22, 2020](https://github.com/microsoft/WSL/issues/4304#issuecomment-632608210)

When using multiple WSL2 distros in parallel, it would have been useful to be able to SSH into any one of them by using different IP numbers. (Personally I use this to manage git repos from Emacs magit running on WSL1 or on Windows native, via TRAMP over ssh.)

As it stands, one either has to run the ssh daemons on different ports, or, IIUC, setup namespaces within each WSL2 distro and have the relevant daemon processes (e.g. ssh) attaching to those.

See e.g. [https://blogs.igalia.com/dpino/2016/04/10/network-namespaces/](https://blogs.igalia.com/dpino/2016/04/10/network-namespaces/) -- setup looks like it could cost some time.

Bottom-line: A more straight-forward, out-of-the-box mechanism whereby different WSL2 distros could be approached via network would be valuable.

👍33JZfi, avinoamsn, stolsma, cyberblackhole, hfellner, ryderjgillen, vineetapte, bulentsoylu, axelfontaine, mthornba, and 23 more reacted with thumbs up emoji

[![@matt335672](media/@matt335672.png)](https://github.com/matt335672) [matt335672](https://github.com/matt335672) mentioned this issue [on Mar 23, 2021](https://github.com/microsoft/WSL/issues/4304#ref-issue-838121239)

[after entering credentials connection does nothing for long time then generates error dialog neutrinolabs/xrdp#1837](https://github.com/neutrinolabs/xrdp/issues/1837)

 Closed

[![@sakai135](media/@sakai135.png)](https://github.com/sakai135) [sakai135](https://github.com/sakai135) mentioned this issue [on May 15, 2021](https://github.com/microsoft/WSL/issues/4304#ref-issue-892217781)

[How to configure with multiple WSL distro? sakai135/wsl-vpnkit#26](https://github.com/sakai135/wsl-vpnkit/issues/26)

 Closed

[![@smeierhofer](media/@smeierhofer.jpg)](https://github.com/smeierhofer)

### 

**[smeierhofer](https://github.com/smeierhofer)** commented [on Nov 12, 2022](https://github.com/microsoft/WSL/issues/4304#issuecomment-1312254712)

If you want to run an SSH server on each WSL distro, could you follow the steps in the link posted by [@craigloewen-msft](https://github.com/craigloewen-msft) to assign additional IP addresses to the network interface card? Then configure the SSH server to bind to only the one IP address (not sure how to do this but I'm sure this is doable). On your 2nd WSL distro you do the same, but configure the SSH server to bind to a different IP address. Then probably you want to add entries into your host file for the IP addresses and then you can SSH into these distros using the hostname you've given in your hosts file. I haven't tried this myself but if it work, it is better way to go than using CGROUPS (networking namespaces) to run an SSH server on each distro. The network namespaces approach seems a better for problems where you want the same IP address but want to isolate more than just use a new IP address or port number.

[![@fzhan](media/@fzhan.png)](https://github.com/fzhan)

### 

**[fzhan](https://github.com/fzhan)** commented [on Nov 22, 2022](https://github.com/microsoft/WSL/issues/4304#issuecomment-1323859678)

I guess by 2022, no one has tried to create three nodes microk8s cluster via WSL2?

Could turn Windows in to a beast.

[![@ismailokta](media/@ismailokta.jpg)](https://github.com/ismailokta)

### 

**[ismailokta](https://github.com/ismailokta)** commented [on Nov 23, 2022](https://github.com/microsoft/WSL/issues/4304#issuecomment-1324533676)

yes I agree, At first, I was happy with WSL2, which is lighter than using virtualbox, it has library isolation, but the network doesn't. really bear it

[![@fl0wm0ti0n](media/@fl0wm0ti0n.png)](https://github.com/fl0wm0ti0n)

### 

**[fl0wm0ti0n](https://github.com/fl0wm0ti0n)** commented [on Nov 27, 2022](https://github.com/microsoft/WSL/issues/4304#issuecomment-1328085477)

i want to scp files to one of my wsl instances but that isn't possible because they have all the same ip address and the address isn't reachable over network... if wsl cant have its own reachable ip its a bit useless sometimes :(

debian -> 172.27.246.30  
debiandev01 -> 172.27.246.30

and "wsl hostname -i" gives for both 127.0.1.1 ...

is it possible to have a reachable ipaddress?

[![@Biswa96](media/@Biswa96.png)](https://github.com/Biswa96)

### 

**[Biswa96](https://github.com/Biswa96)** commented [on Nov 27, 2022](https://github.com/microsoft/WSL/issues/4304#issuecomment-1328086225)

Instead of using network, wouldn't it be possible to use shared mount points, for example, /mnt/wsl or /mnt/wslg ?

Also it is possible to mount one distribution to another, both are running. I forgot the exact name of that feature.