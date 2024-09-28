wsl --list --online 查看可在线安装的linux系统发行版

wsl --install （安装ubantu，默认）
其他版本可通过wsl --install -d <Distribution Name>安装


报错“安装其中一个文件系统时出现错误。有关详细信息，请运行’dmesg’。
”
运行以下代码：wsl --update 后运行wsl --shutdown重启wsl即可