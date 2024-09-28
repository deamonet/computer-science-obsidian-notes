
2018-11-04

### 一、简介

- FastCopy是Windows上最快的复制/删除软件
    
- 支持UNICODE和超过最大长度（260个字符）的文件路径名
    
- 使用多线程进行读、写、校验，重叠(异步)IO、直接IO，提供设备最快的速度
    
- 支持类似通配符的包含或排除过滤器
    
- 运行速度快而且不会独占资源
    

### 二、常用功能

#### 1、Source/DestDir

- `Source`可以指定多个文件或目录，中间用分号分隔
    
- 支持文件路径的拖拽
    

![](http://www.albertbamboo.cn/assets/images/software/fastcopy/drag-paths.gif)

- 如果DestDir的最后一个符号是`\`，则会将Source目录本身也拷贝过去(DestDir\SourceDir\Contents_of_SourceDir)
    
- 如果Source的路径中是多个文件(或目录)，且DestDir的最后没有`\`符号，拷贝的结果同上
    

#### 2、模式

|   |   |
|---|---|
|Diff(No Overwrite)|拷贝目标目录中不存在相同文件名的文件|
|Diff(Size/date)|默认模式，拷贝大小或日期不同，或者目标目录中没有的文件|
|Diff(Newer)|只拷贝源目录中文件时间较新的，或者目标目录中没有的文件|
|Copy(Overwrite)|拷贝所有的文件，如果存在相同的则覆盖|
|Sync(Size/date)|与默认模式类似，除此之外，会删除目标文件中在源文件中不存的文件(目录的同步)|
|Move(Overwrite)|移动文件或目录(剪切)|
|Delete|删除所有的文件或目录|

#### 3、Job Manage

在`JobMng--Add/Modify/Del Job...`菜单中，可以将当前的复制路径添加为任务，或修改原有的任务，也可以删除指定的任务。

![](http://www.albertbamboo.cn/assets/images/software/fastcopy/job-manage-src-dest.png)

![](http://www.albertbamboo.cn/assets/images/software/fastcopy/register-modify-del-job.png)

#### 4、过滤

- 通配符

|   |   |
|---|---|
|*|零个或多个任意字符|
|?|任意一个字符|
|[abc]|“abc”中的其中一个字符|
|[!abc]或[^abc]|除”abc”之外的任意一个字符|
|[a-z]|“abc…xyz”中的任意一个字符|
|\|路径分隔符或转义字符||

- 包含

只复制满足条件的文件或目录。可以使用**分号分隔多个过滤条件**；如果要指定目录，需要在目录名末尾添加`\`。

- 排除

不复制满足过滤条件的文件或目录

- 相对路径

如果过滤条件不是以`\`开头的，则作为相对路径过滤。例如：

|   |   |
|---|---|
|Source：|C:\dir\||
|Include：|subdir[1-9]\xxx|

则`C:\dir\subdir2\xxx\` 和 `C:\dir\dir\subdir3\xxx\`目录满足包含条件；

|   |   |
|---|---|
|Source：|C:\dir\||
|Include：|subdir[1-9]\file.*|

则`C:\dir\subdir5\file.html` 或 `C:\dir\zzz\subdir9\file.txt`都满足包含条件。

- 起始路径

如果过滤条件是以`\`开头，则作为起始路径过滤。例如：

|   |   |
|---|---|
|Source：|C:\dir\||
|Include：|\subdir[1-9]\xxx|

则`C:\dir\subdir2\xxx\` 和 `C:\dir\subdir3\xxx\`满足条件，而`C:\dir\dir2\subdir2\xxx\`不满足。

|   |   |
|---|---|
|Source：|C:\dir\||
|Include：|\subdir[1-9]\file.*|

则`C:\dir\subdir9\file.txt`满足条件，而`C:\dir\dir2\subdir9\file.txt`不满足。

#### 5、扩展过滤

可以在菜单`Option -> Show Extended filter`中开启时间和大小的过滤。

- 日期格式

|   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|
|绝对格式|“YYYYMMDD”，例如：”20181101”||||||
|相对格式|”+|- number W|D|h|m|s”，W、D、h、m、s分别表示周、天、时、分、秒，例如：”-12h”(大小写敏感)|

- FromDate

只拷贝指定日期之后的文件，例如：”20181010”或”-10D”(10天前)

- ToDate

只拷贝指定日期之前的文件

- MinSize

只拷贝文件大小大于指定大小的文件，可以使用K/M/G/T等单位。

- MaxSize

只拷贝文件大小小于指定大小的文件

##### 附：

- [FastCopy官网](https://fastcopy.jp/en/)
    
- Direct I/O
    

直接I/O是一种不使用内核中的整个缓存而直接将I/O发送到磁盘的方式。

- [Overlapped I/O(Asynchronous I/O)](https://baike.baidu.com/item/%E5%BC%82%E6%AD%A5IO/6018433)