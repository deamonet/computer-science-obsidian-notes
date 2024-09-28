环境：windows 下的 Ubuntu
windows下的C D等盘符在安装WSL后自动挂载在 /mnt/目录下，U盘插入电脑后有些格式（例如：FAT32格式）是无法自动挂载的，（如果自动挂载也是默认挂载在/mnt/目录下），无法自动挂载的情况下，需要手动挂载。
1.打开ubuntu 在/mnt/下创建一个文件夹(F)，以便将U盘挂载在此
sudo mkdir /mnt/F
2.将U盘挂载在上面创建的文件夹F中,U盘插入电脑之后在电脑的盘符为F，需要适合用drvfs进行挂载
mount -t drvfs F: /mnt/F
3.如果想在windows下正常弹出，需要先将umount
umount /mnt/f

-   -t：指定档案系统的型态，通常不必指定。mount 会自动选择正确的型态。

首先要了解 WSL 中的[两种文件系统](https://p3terx.com/go/aHR0cHM6Ly9ibG9ncy5tc2RuLm1pY3Jvc29mdC5jb20vd3NsLzIwMTYvMDYvMTUvd3NsLWZpbGUtc3lzdGVtLXN1cHBvcnQv)：

-   **VolFs**
    
    着力于在 Win­dows 文件系统上提供完整的 Linux 文件系统特性，通过各种手段实现了对 In­odes、Di­rec­tory en­tries、File ob­jects、File de­scrip­tors、Spe­cial file types 的支持。比如为了支持 Win­dows 上没有的 In­odes，VolFs 会把文件权限等信息保存在文件的 NTFS Ex­tended At­trib­utes 中。
    
    WSL 中的 `/` 使用的就是 VolFs 文件系统。
    
-   **DrvFs**
    
    着力于提供与 Win­dows 文件系统的互操作性。与 VolFs 不同，为了提供最大的互操作性，DrvFs 不会在文件的 NTFS Ex­tended At­trib­utes 中储存附加信息，而是从 Win­dows 的文件权限（Ac­cess Con­trol Lists，就是你右键文件 > 属性 > 安全选项卡中的那些权限配置）推断出该文件对应的的 Linux 文件权限。
    
    所有 Win­dows 盘符挂载至 WSL 下的 `/mnt` 时都是使用的 DrvFs 文件系统。
    

简单来说就是 WSL 对 `/` 目录下的文件拥有完整的控制权，而 `/mnt` 目录中的文件无法被 WSL 完全控制（可修改数据，无法真实的修改权限）。WSL 对 `/mnt` 目录中权限的修改不会直接记录到文件本身，而在 Win­dows 下对文件权限的修改直接可作用到 WSL 。关于权限在微软开发者博客中有[更详细的说明](https://p3terx.com/go/aHR0cHM6Ly9kZXZibG9ncy5taWNyb3NvZnQuY29tL2NvbW1hbmRsaW5lL2NobW9kLWNob3duLXdzbC1pbXByb3ZlbWVudHMv)。


# Linux mount命令

 [![Linux 命令大全](media/Linux_命令大全.gif) Linux 命令大全](https://www.runoob.com/linux/linux-command-manual.html)

Linux mount命令是经常会使用到的命令，它用于挂载Linux系统外的文件。

### 语法

mount [-hV]
mount -a [-fFnrsvw] [-t vfstype]
mount [-fnrsvw] [-o options [,...]] device | dir
mount [-fnrsvw] [-t vfstype] [-o options] device dir

**参数说明：**

-   -V：显示程序版本
-   -h：显示辅助讯息
-   -v：显示较讯息，通常和 -f 用来除错。
-   -a：将 /etc/fstab 中定义的所有档案系统挂上。
-   -F：这个命令通常和 -a 一起使用，它会为每一个 mount 的动作产生一个行程负责执行。在系统需要挂上大量 NFS 档案系统时可以加快挂上的动作。
-   -f：通常用在除错的用途。它会使 mount 并不执行实际挂上的动作，而是模拟整个挂上的过程。通常会和 -v 一起使用。
-   -n：一般而言，mount 在挂上后会在 /etc/mtab 中写入一笔资料。但在系统中没有可写入档案系统存在的情况下可以用这个选项取消这个动作。
-   -s-r：等于 -o ro
-   -w：等于 -o rw
-   -L：将含有特定标签的硬盘分割挂上。
-   -U：将档案分割序号为 的档案系统挂下。-L 和 -U 必须在/proc/partition 这种档案存在时才有意义。
-   -t：指定档案系统的型态，通常不必指定。mount 会自动选择正确的型态。
-   -o async：打开非同步模式，所有的档案读写动作都会用非同步模式执行。
-   -o sync：在同步模式下执行。
-   -o atime、-o noatime：当 atime 打开时，系统会在每次读取档案时更新档案的『上一次调用时间』。当我们使用 flash 档案系统时可能会选项把这个选项关闭以减少写入的次数。
-   -o auto、-o noauto：打开/关闭自动挂上模式。
-   -o defaults:使用预设的选项 rw, suid, dev, exec, auto, nouser, and async.
-   -o dev、-o nodev-o exec、-o noexec允许执行档被执行。
-   -o suid、-o nosuid：
-   允许执行档在 root 权限下执行。
-   -o user、-o nouser：使用者可以执行 mount/umount 的动作。
-   -o remount：将一个已经挂下的档案系统重新用不同的方式挂上。例如原先是唯读的系统，现在用可读写的模式重新挂上。
-   -o ro：用唯读模式挂上。
-   -o rw：用可读写模式挂上。
-   -o loop=：使用 loop 模式用来将一个档案当成硬盘分割挂上系统。

### 实例

将 /dev/hda1 挂在 /mnt 之下。

#mount /dev/hda1 /mnt

将 /dev/hda1 用唯读模式挂在 /mnt 之下。

#mount -o ro /dev/hda1 /mnt

将 /tmp/image.iso 这个光碟的 image 档使用 loop 模式挂在 /mnt/cdrom之下。用这种方法可以将一般网络上可以找到的 Linux 光 碟 ISO 档在不烧录成光碟的情况下检视其内容。

#mount -o loop /tmp/image.iso /mnt/cdrom