[#]: collector: (lujun9972)
[#]: translator: (geekpi)
[#]: reviewer: ( )
[#]: publisher: ( )
[#]: url: ( )
[#]: subject: (Linux sudo flaw can lead to unauthorized privileges)
[#]: via: (https://www.networkworld.com/article/3446036/linux-sudo-flaw-can-lead-to-unauthorized-privileges.html)
[#]: author: (Sandra Henry-Stocker https://www.networkworld.com/author/Sandra-Henry_Stocker/)

Linux sudo 漏洞可能导致未经授权的特权
======
在 Linux 中利用新发现的 sudo 漏洞可以使某些用户以 root 身份运行命令，尽管对此还有所限制。

[**sudo**][1] 命令中最近发现了一个严重漏洞，如果被利用，那么即使在 **/etc/sudoers** 文件中明确禁止了该用户，它们也可以以 root 身份运行命令。。

将 **sudo** 更新到版本 1.8.28 应该可以解决该问题，因此鼓励 Linux 管理员尽快这样做。

如何利用此漏洞取决于  **/etc/sudoers** 中授予的特定权限。例如，一条规则允许用户以除 root 用户之外的任何用户身份来编辑文件，这实际上将允许该用户也以 root 用户身份来编辑文件。在这种情况下，该漏洞可能会导致非常严重的问题。

要让用户能够利用此漏洞，需要在 **/etc/sudoers ** 中为**用户**分配权限，以使该用户可以像其他用户一样运行命令，并且该漏洞仅限于以这种方式分配的命令特权。

此问题影响 1.8.28 之前的版本。要检查你的 sudo 版本，请使用以下命令：

```
$ sudo -V
Sudo version 1.8.27     <===
Sudoers policy plugin version 1.8.27
Sudoers file grammar version 46
Sudoers I/O plugin version 1.8.27
```

该漏洞已在“常见漏洞和披露”数据库中分配了编号 [CVE-2019-14287][4]。它的风险是，任何被指定能以任意用户运行单个命令的用户，即使被明确禁止以 root 身份运行，它都能逃脱限制。

下面这些行让 “jdoe” 能够以除了 root 用户之外的其他身份使用 **vi**编辑文件（**!root**表示“非 root”），同时 nemo有权运行以除了 root 身份以外的任何用户使用 **id** 命令：

```
# affected entries on host "dragonfly"
jdoe dragonfly = (ALL, !root) /usr/bin/vi
nemo dragonfly = (ALL, !root) /usr/bin/id
```

但是，由于存在漏洞，这些用户中的任何一个都将能够绕过限制并编辑文件，或者也可以 root 用户身份运行 **id** 命令。


攻击者可以通过指定用户 ID 为 “-1” 或 “4294967295” 来以 root 身份运行命令。

响应 “1” 表明该命令以 root 身份运行（显示 root 的用户 ID）。

苹果信息安全团队的 Joe Vennix 找到并分析该问题。

在 [Facebook][5] 和 [LinkedIn][6] 加入 Network World 社区，评论热门主题。

--------------------------------------------------------------------------------

via: https://www.networkworld.com/article/3446036/linux-sudo-flaw-can-lead-to-unauthorized-privileges.html

作者：[Sandra Henry-Stocker][a]
选题：[lujun9972][b]
译者：[geekpi](https://github.com/geekpi)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创编译，[Linux中国](https://linux.cn/) 荣誉推出

[a]: https://www.networkworld.com/author/Sandra-Henry_Stocker/
[b]: https://github.com/lujun9972
[1]: https://www.networkworld.com/article/3236499/some-tricks-for-using-sudo.html
[4]: http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-14287
[5]: https://www.facebook.com/NetworkWorld/
[6]: https://www.linkedin.com/company/network-world