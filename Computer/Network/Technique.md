
+ A（Address）记录是用来指定域名所对应的IP地址的记录。

+ MX（Mail Exchanger）记录是邮件交换记录，它指向一个邮件服务器，用于电子邮件系统发邮件时根据收信人的地址后缀来定位邮件服务器。
>  例如，当Internet上的某用户要发一封信给 webmaster@meibu.com 时，该用户的计算机将通过DNS查找meibu.com这个域名的MX记录，如果MX记录存在，用户计算机就将邮件发送到MX记录所指定的邮件服务器上。

+ CNAME（Canonical \[kə'nɒnɪkl\] Name）记录，通常称别名指向。
> 用户可以定义一个主机别名，比如ftp.meibu.com，用来指向一个主机，比如www.meibu.com，那么访问ftp.meibu.com，将转向到www.meibu.com

+ NS（Name Server）记录是域名服务器记录，用来指定该域名由哪个DNS服务器来进行解析。
> 如果您在每步申请了顶级域名或独立域名的动态解析服务，需要到您的域名注册商那里，将您的域名的NS记录设为每步的DNS服务器。我们的DNS服务器是：dns1.meibu.com和dns2.meibu.com，IP地址分别是202.104.237.179和61.129.47.188。NS设置后需要一、两天后才能生效，您可以用下面的办法查询是否已经生效。
泛域名是指一个域名下的所有子域名都被解析到同一个IP地址上。例如：*.meibu.com都解析到meibu.com

Pre-set maximums 中的 RX/TX 值为该网卡的 Buffer size 最大值；
Current hardware settings 中 RX/TX 值代表该网卡当前的 Buffer size 大小。
所以，设置的 Current hardware settings 的 RX/TX 值必须在 Pre-set maximums 的限制之内。
