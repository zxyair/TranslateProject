[#]: subject: (How to Setup Internet in CentOS, RHEL, Rocky Linux Minimal Install)
[#]: via: (https://www.debugpoint.com/2021/06/setup-internet-minimal-install-server/)
[#]: author: (Arindam https://www.debugpoint.com/author/admin1/)
[#]: collector: (lujun9972)
[#]: translator: (geekpi)
[#]: reviewer: ( )
[#]: publisher: ( )
[#]: url: ( )

如何在 CentOS、RHEL、Rocky Linux 最小化安装中设置互联网
======
在最小安装的服务器中，设置互联网或网络是非常容易的。在本指南中，我们将解释如何在 CentOS、RHEL、Rocky Linux 最小安装中设置互联网或网络。

当你安装了任何服务器发行版的最小安装，你就没有任何 GUI 或桌面环境来设置你的网络或互联网。因此，当你只能使用终端时，知道如何设置互联网是很重要的。NetworkManager 工具提供了必要的工具和 systemd 服务来完成这项工作。以下是具体方法。

### 在 CentOS、RHEL、Rocky Linux 最小化安装中设置互联网

  * 完成安装后，启动服务器终端。理想情况下，你应该会看到提示。使用 root 或 admin 账户登录。


  * 然后，首先尝试使用 nmcli 检查网络接口的状态和细节。nmcli 是一个控制 NetworkManager 服务的命令行工具。使用以下命令进行检查。



```
nmcli device status
```

这将显示设备名称、状态等。

![nmcli device status][1]

  * 运行工具 `nmtui` 来配置网络接口。[nmtui][2] 是 NetworkManager 工具的一部分，它为你提供了一个漂亮的用户界面来配置网络。这是 NetworkManager-tui 包的一部分，当你完成最小服务器的安装后，它应该默认安装。



```
nmtui
```

  * 在 nmtui 窗口中点击编辑一个连接。



![nmtui – Select options][3]

  * 选择接口名称



![Select Interface to Edit][4]

  * 在编辑连接窗口，为 IPv4 和 IPv6 选择自动。并选择自动连接。完成后按 OK。



![nmtui – Edit Connection][5]

  * 使用下面的命令，通过 [systemd systemctl][6] 重新启动 NetworkManager 服务。



```
systemctl restart NetworkManager
```

  * 如果一切顺利，你应该在 CentOS、RHEL、Rocky Linux 服务器的最小安装中连接到网络和互联网。前提是你的网络有互联网连接。你可以用 ping 来验证它是否正常。



![setup internet minimal server – CentOS Rocky Linux RHEL][7]

### 额外技巧：在最小服务器中设置静态 IP

当你把网络配置设置为自动，当你连接到互联网时，接口会动态地分配 IP。在某些情况下，当你建立一个局域网 （LAN） 时，你可能想给你的网络接口分配静态 IP。这超级简单。

打开你的网络的网络配置脚本。根据你的设备修改高亮部分。

```
vi /etc/sysconfig/network-scripts/ifcfg-ens3
```

在上面的文件中，用 IPADDR 属性添加你想要的 IP 地址。保存该文件。

```
IPADDR=192.168.0.55
```

在 `/etc/sysconfig/network` 中为你的网络添加网关。

```
NETWORKING=yes
HOSTNAME=debugpoint
GATEWAY=10.1.1.1
```

在位于 `/etc/resolv.conf` 的 resolv.conf 中添加任意公共 DNS 服务器。

nameserver 8.8.8.8
nameserver 8.8.4.4

并重新启动网络服务。

```
systemctl restart NetworkManager
```

这样就完成了静态 IP 的设置。你也可以使用 `ip addr` 命令检查 IP 的详细信息。

我希望这个指南能帮助你在你的最小服务器中设置网络、互联网和静态 IP。如果你有任何问题，请在评论区告诉我。

* * *

--------------------------------------------------------------------------------

via: https://www.debugpoint.com/2021/06/setup-internet-minimal-install-server/

作者：[Arindam][a]
选题：[lujun9972][b]
译者：[geekpi](https://github.com/geekpi)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创编译，[Linux中国](https://linux.cn/) 荣誉推出

[a]: https://www.debugpoint.com/author/admin1/
[b]: https://github.com/lujun9972
[1]: https://www.debugpoint.com/blog/wp-content/uploads/2021/06/nmcli-device-status.jpg
[2]: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/networking_guide/sec-configuring_ip_networking_with_nmtui
[3]: https://www.debugpoint.com/blog/wp-content/uploads/2021/06/nmtui-Select-options.jpg
[4]: https://www.debugpoint.com/blog/wp-content/uploads/2021/06/Select-Interface-to-Edit.jpg
[5]: https://www.debugpoint.com/blog/wp-content/uploads/2021/06/nmtui-Edit-Connection.jpg
[6]: https://www.debugpoint.com/2020/12/systemd-systemctl-service/
[7]: https://www.debugpoint.com/blog/wp-content/uploads/2021/06/setup-internet-minimal-server-CentOS-Rocky-Linux-RHEL.jpg
