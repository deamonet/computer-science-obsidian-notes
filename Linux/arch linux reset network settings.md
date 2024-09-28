在 Arch Linux 中重置网络设置的步骤如下：

1. 首先，打开终端并以管理员身份运行。
    
2. 输入以下命令来停止网络服务：
    

arduino

复制代码

`sudo systemctl stop NetworkManager`

3. 输入以下命令来删除当前的网络配置文件：

bash

复制代码

`sudo rm /etc/udev/rules.d/70-persistent-net.rules`

4. 输入以下命令来重置网络设置：

复制代码

`sudo systemctl restart systemd-networkd`

5. 然后，输入以下命令来启动网络服务：

sql

复制代码

`sudo systemctl start NetworkManager`

这些步骤将重置您的网络设置，并将其还原为默认设置。请注意，这可能会导致您需要重新配置您的网络连接。如果您遇到任何问题，您可以尝试重新启动您的计算机并检查是否修复了问题。