2018-11-29[笔记本](https://blog.skk.moe/categories/%E7%AC%94%E8%AE%B0%E6%9C%AC/)约 1.9 千字

首先虽然题目包含 Ubuntu 但是实际上苏卡卡解决的是 WSL 的代理问题的 ~~WSL 是最好的 Linux 发行版~~；而且 Ubuntu 超级棒的 ~~不出内部错误的话~~！苏卡卡的方案也是通用于几乎所有 Linux 的。

在 Windows 上使用全局代理亦或是透明代理真是太困难了。我之前在用 SSTap 实现全局代理；后来 Clash 和 Clash for Windows 出来以后就直接去用 Clash 了 ~~界面漂亮才是第一生产力！~~ 我就直接把 ~~难看的~~ SSTap 丢掉了。但是 Clash 只是一个代理程序，并不能提供全局代理的方案。  
好在 Windows 上实现全局代理或者分应用代理还有一个常用的方案——Proxifier，我可以通过 Proxifier 把绝大部分应用的流量转发到 Clash 上面去，Windows 给 UWP 应用做的应用隔离则可以使用 Fiddler 的 `WinConfig` 工具解除掉。

但关键是 WSL。之前使用 SSTap 直接接管 Windows 除了 Windows 系统进程以外的网络层，当然也包括 WSL。Proxifier 并不能实现对 WSL 启用代理了，所以用 Proxifier 就得解决 WSL 也走代理的方法。

在 Google 上找遍了「Linux 全局代理」，相对最完美的方法是使用 Proxychains，但每个指令前面都要带个 `proxychains` 真的太不方便了；使用 `iptables` 的透明代理方案在 WSL 上面也并不适用（WSL 和 Windows 两者是相互依赖的、我不想在 Ubuntu 上设置 `iptables` 结果把宿主 Windows 炸掉）。  
既然没有，那就干脆不要了！直接在终端配置 proxy，反正目前在 WSL 上使用的应用和环境并不多。

---

给终端设置代理超简单的，直接一句命令搞定：

```bash
export ALL_PROXY="socks5://127.0.0.1080"
```

这样 `curl` `wget` 是都走代理了，但是 `git` `npm` `yarn` 这些不知为何不跟随终端的环境变量设置。一种解决方案是分别设置：

```bash
git config --global http.proxy "socks5://127.0.0.1080"
git config --global https.proxy "socks5://127.0.0.1080"
npm config set proxy "http://127.0.0.1090"
npm config set https-proxy "http://127.0.0.1090"
yarn config set proxy "http://127.0.0.1090"
yarn config set https-proxy "http://127.0.0.1090"
```

但是苏卡卡发现了一个奇怪的解决方法，就是除了设置 `ALL_RPOXY` 以外再添加一个 `all_proxy` ：

```bash
export all_proxy="socks5://127.0.0.1080"
```

这样不仅 `git` `npm` `yarn` ~~这些读取环境变量不支持大小写的辣鸡~~，其它绝大部分终端应用程序不走代理的问题就都解决了~

不过包管理器 APT 就更麻烦了。如果需要设置 APT 走代理，需要修改 `/etc/apt/apt.conf`：

```bash
echo -e "Acquire::http::Proxy \"http://127.0.0.1:1090\";" | sudo tee -a /etc/apt/apt.conf > /dev/null
echo -e "Acquire::https::Proxy \"http://127.0.0.1:1090\";" | sudo tee -a /etc/apt/apt.conf > /dev/null
```

---

敲了好一会键盘以后苏卡卡想起了一个问题——如果要解除代理应该怎么办。。。

于是苏卡卡在 `.zshrc` 里面把解除代理和启用代理写成了两个 function：`proxy()` 和 `noproxy()`，每次只要执行 `proxy` 就可以启用代理、执行 `noproxy` 解除代理，近似实现「一键操作」。而且，还可以在 function 中加上 `curl ip.gs` 来测试当前 IP。

Talk is cheap, here are the codes:

```bash
proxy () {
  export ALL_PROXY="socks5://127.0.0.1:1080"
  export all_proxy="socks5://127.0.0.1:1080"
  echo -e "Acquire::http::Proxy \"http://127.0.0.1:1090\";" | sudo tee -a /etc/apt/apt.conf > /dev/null
  echo -e "Acquire::https::Proxy \"http://127.0.0.1:1090\";" | sudo tee -a /etc/apt/apt.conf > /dev/null
  curl https://ip.gs
}

noproxy () {
  unset ALL_PROXY
  unset all_proxy
  sudo sed -i -e '/Acquire::http::Proxy/d' /etc/apt/apt.conf
  sudo sed -i -e '/Acquire::https::Proxy/d' /etc/apt/apt.conf
  curl https://ip.gs
}
```

> 稍加修改也可以写进 `.bashrc` 之中

苏卡卡最终的版本用 `read` 实现了读取输入来获取代理地址、而不是直接使用预置 `127.0.0.1:1080` 的方案。代码开在 [这里](https://github.com/SukkaW/dotfiles/blob/master/_zshrc/proxy.rc)。如果各位路过的大佬有什么更高明的 Linux 全局代理或者 WSL 由 Windows 接管代理的方法，请务必点醒菜鸡苏卡卡。

Ubuntu「一键」设置代理

[https://blog.skk.moe/post/enable-proxy-on-ubuntu/](https://blog.skk.moe/post/enable-proxy-on-ubuntu/)

本文作者

Sukka

发布于

2018-11-29

许可协议

[CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)

转载或引用本文时请遵守许可协议，注明出处、不得用于商业用途！

本文最后更新于 1523 天前，文中所描述的信息可能已发生改变

[# 笔记本](https://blog.skk.moe/categories/%E7%AC%94%E8%AE%B0%E6%9C%AC/)[# WSL](https://blog.skk.moe/tags/WSL/)[# Ubuntu](https://blog.skk.moe/tags/Ubuntu/)[# Linux](https://blog.skk.moe/tags/Linux/)[# 全局代理](https://blog.skk.moe/tags/%E5%85%A8%E5%B1%80%E4%BB%A3%E7%90%86/)