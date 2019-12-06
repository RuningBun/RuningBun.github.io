---
layout: post
title: "centos7 搭建 svn"
categories: centos
tags: centos subversion
---

* content
{:toc}

1.环境
centos7

2.安装svn
yum -y install subversion

3.配置

建立版本库目录
mkdir /www/svndata

svnserve -d -r /www/svndata

4.建立版本库

创建一个新的Subversion项目
svnadmin create /www/svndata/oplinux

配置允许用户rsync访问
cd /www/svndata/oplinux/conf

vi svnserve.conf
anon-access=read
auth-access=write
password-db=passwd

注：修改的文件前面不能有空格，否则启动svn server出错

```
    vi auth

    [groups]

    # harry_and_sally = harry,sally

    manager = sally

    # [repository:/baz/fuz]
    # @harry_and_sally = rw
    # * = r
    [/]
    @manager=rw
    *=r

    vi passwd
    [users]
    #<用户1> = <密码1>
    #<用户2> = <密码2>
    sally=123456

```

启动服务器
 svnserve -d -r /www/svndata/oplinux
5.svn服务端口3690要放开，否则会无法访问；

请依次检查下面各项
a，服务器有没有运行，有没有打开相应端口
如果服务器是svnserve，检查有没有运行svnserve，有没有打开3690端口
检查时可以在服务器运行netstat -an看看相应端口是否在LISTEN
b，防火墙有没有开放相应端口
c，客户端是否可以连接服务器的相应端口
使用命令telnet 服务器IP 相应端口
如：telnet 192.168.0.1 3690

如果没有打开，centos7默认使用firewall取代了iptables ，需要如下操作。

编辑配置文件

vi /etc/sysconfig/iptables #编辑防火墙配置文件

在下面的后面增加你需要的端口号

-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT#默认开启22的sshd端口

-A INPUT -m state --state NEW -m tcp -p tcp --dport 你需要的端口号 -j ACCEPT

保存退出后

systemctl restart iptables.service #最后重启防火墙使配置生效

即可解决


6.实现SVN与WEB同步,可以CO一个出来,也可以直接配在仓库中

1)设置WEB服务器根目录为/www/webroot

2)checkout一份SVN

svn co svn://localhost/oplinux /www/webroot

修改权限为WEB用户

chown -R apache:apache /www/webroot/oplinux

3)建立同步脚本

cd /www/svndata/oplinux/hooks/

cp post-commit.tmpl post-commit

编辑post-commit,在文件最后添加以下内容
```
export LANG=en_US.UTF-8
SVN=/usr/bin/svn
WEB=/www/webroot/
$SVN update $WEB –username rsync –password rsync
chown -R apache:apache $WEB
```
增加脚本执行权限
```chmod +x post-commit```