[![](media/head.jpg)](https://blog.kaibro.tw/)

# [KaiBro](https://blog.kaibro.tw/)

-   [主頁](https://blog.kaibro.tw/)
-   [所有文章](https://blog.kaibro.tw/archives)
-   [關於我](http://cv.kaibro.tw/)

[github](https://github.com/w181496 "github") [rss](https://blog.kaibro.tw/atom.xml "rss") [facebook](https://www.facebook.com/w181496 "facebook") [twitter](https://twitter.com/KAIKAIBRO "twitter")

[BugBounty](https://blog.kaibro.tw/tags/BugBounty/) [CTF](https://blog.kaibro.tw/tags/CTF/) [DEFCON](https://blog.kaibro.tw/tags/DEFCON/) [Java](https://blog.kaibro.tw/tags/Java/) [Linux](https://blog.kaibro.tw/tags/Linux/) [NCU](https://blog.kaibro.tw/tags/NCU/) [NTU](https://blog.kaibro.tw/tags/NTU/) [Pwn](https://blog.kaibro.tw/tags/Pwn/) [Web](https://blog.kaibro.tw/tags/Web/) [筆記](https://blog.kaibro.tw/tags/筆記/) [開箱](https://blog.kaibro.tw/tags/開箱/) [開箱文](https://blog.kaibro.tw/tags/開箱文/)

Web security菜雞

[2016-11-07](https://blog.kaibro.tw/2016/11/07/Linux-Kernel編譯-Ubuntu/)

# Linux Kernel編譯+新增System call(Ubuntu)

-   [NCU](https://blog.kaibro.tw/2016/11/07/Linux-Kernel%E7%B7%A8%E8%AD%AF-Ubuntu/tags/NCU/)
-   [Linux](https://blog.kaibro.tw/2016/11/07/Linux-Kernel%E7%B7%A8%E8%AD%AF-Ubuntu/tags/Linux/)
-   [筆記](https://blog.kaibro.tw/2016/11/07/Linux-Kernel%E7%B7%A8%E8%AD%AF-Ubuntu/tags/筆記/)

## [](https://blog.kaibro.tw/2016/11/07/Linux-Kernel%E7%B7%A8%E8%AD%AF-Ubuntu/#u524D_u60C5_u63D0_u8981 "前情提要")前情提要

因Linux Operating System課的project需要，人生第一次編Kernel就獻給他惹，所以怕忘就順手筆記一下。

## [](https://blog.kaibro.tw/2016/11/07/Linux-Kernel%E7%B7%A8%E8%AD%AF-Ubuntu/#u74B0_u5883 "環境")環境

使用Ubuntu 16.04 LTS i386 Desktop  
要編的Kernel是3.10版

## [](https://blog.kaibro.tw/2016/11/07/Linux-Kernel%E7%B7%A8%E8%AD%AF-Ubuntu/#u904E_u7A0B "過程")過程

一開始當然要先裝好Linux環境(我是裝Ubuntu)  
然後決定你要編的Kernel，並載下來解壓縮  
(Kernel source可以到[https://www.kernel.org](https://www.kernel.org) 下載)  
我這邊解壓縮出來後是linux-3.10.104/

1

$ cd linux-3.10.104/

進去之後，要新增一個system call  
創一個資料夾用來放我們system call的code

1

2

$ mkdir mycall

$ cd mycall

新增一個檔案helloworld.c，就是我們system call的程式

1

$ vim helloworld.c

1

2

3

4

5

#include <linux/kernel.h>

asmlinkage int sys_helloworld(void) {

    printk("kaibro ininder\n");

    return 1;

}

再來新增Makefile，用來確保我們的程式會被編譯進Kernel

1

2

$ vim Makefile

obj-y := helloworld.o

再來回上一層目錄，修改Makefile

1

2

3

4

5

6

$ cd ..

$ vim Makefile

core-y += kernel/ mm/ fs/ ipc/ security/ crypto/ block/

找到這行，在最後面新增 mycall/

core-y += kernel/ mm/ fs/ ipc/ security/ crypto/ block/ mycall/

這是為了告訴它，我們新的system call的source files在mycall資料夾裡

再來我們要新增我們的system call到system call table裡

1

2

3

4

5

// 如果是64位元系統，就是syscall_64.tbl

$ vim arch/x86/syscalls/syscall_32.tbl

在最後面新增一行，原本system call只到350號，所以這邊就填351號

351   i386  helloworld  sys_helloworld

再來修改system call header

1

2

3

4

5

6

7

$ vim include/linux/syscalls.h

在最下面(#endif前)新增一行

asmlinkage int helloworld(void);

這定義了我們system call的prototype

asmlinkage代表我們的參數都可以在stack裡取用

接著編譯kernel前要裝一些套件

1

$ sudo apt-get install libncurses5-dev

還有設定檔

1

2

3

$ sudo make menuconfig

// 這邊我都用預設的

$ sudo make oldconfig

最後就開始漫長的compile了！

1

$ sudo make

編完之後，就安裝到我們系統上

1

$ sudo make modules_install install

裝好之後沒問題，就重開機，用我們新的kernel開機

但是grub選單，預設好像是關的

所以我們要先把它打開，再重開機

重開時記得選我們編的kernel

1

2

3

4

5

6

7

8

$ sudo vim /etc/default/grub

像我一樣找到下面這兩行，前面加#，把他們註解掉

#GRUB_HIDDEN_TIMEOUT=0

#GRUB_HIDDEN_TIMEOUT_QUIET=true

然後更新

$ sudo update-grub

之後重開機就能看到選單了

最後寫個小程式來檢驗是否成功新增system call

1

2

3

4

5

6

7

#include <syscall.h>

#include <sys/types.h>

int main()

{

    int a = syscall(351);

    return 0;

}

然後執行之後，用以下這指令看kernel有沒有輸出我們想要的訊息

1

$ dmesg

如果有看到”kaibro ininder”，就代表成功惹～

## [](https://blog.kaibro.tw/2016/11/07/Linux-Kernel%E7%B7%A8%E8%AD%AF-Ubuntu/#u5F8C_u8A18 "後記")後記

其實一開始我是裝Fedora  
但不知為何總是一直失敗  
一開始是虛擬機空間開太小  
後來好不容易重灌好，也編完，卻莫名無法開機  
想說是編太新版的Kernel炸掉，結果重編個相同版本的Kernel還是一樣  
(前前後後大概編了6次吧….)  
之後就果斷改用Ubuntu…

[**<**

Linux project - PGD present bit

](https://blog.kaibro.tw/2016/11/10/Linux-project-PGD-present-bit/)[

DJ DAO FB9 Jubeat控制器開箱

**>**](https://blog.kaibro.tw/2016/02/07/DJ-DAO-FB9-Jubeat控制器開箱/)

Share to:  [](http://www.jiathis.com/share)

© 2022 KaiBro

[Hexo](http://hexo.io/) Theme [Yilia](https://github.com/litten/hexo-theme-yilia) by Litten