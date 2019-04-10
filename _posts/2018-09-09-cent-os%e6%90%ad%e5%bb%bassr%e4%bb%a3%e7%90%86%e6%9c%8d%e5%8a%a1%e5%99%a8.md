---
layout: post
title: Cent OS搭建SSR代理服务器
date: 2018-09-09 21:21
author: chuwei
comments: true
categories: [未分类]
---
购买VPS：<a href="https://www.vultr.com/?ref=7530523">https://www.vultr.com/?ref=7530523</a>

部署SSR代码：

设置密码
设置端口（防火墙要记得开放）
设置加密 默认
设置的协议插件 3
设置的混淆插件 6
自动部署
完成后记下账号密码等信息

yum -y install wget
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh
chmod +x shadowsocksR.sh
./shadowsocksR.sh 2&gt;&amp;1 | tee shadowsocksR.log

# ./shadowsocksR.sh 2&gt;&amp;1 | tee shadowsocksR.log

#############################################################
# One click Install ShadowsocksR Server #
# Intro: https://shadowsocks.be/9.html #
# Author: Teddysun &lt;i@teddysun.com&gt; #
# Github: https://github.com/shadowsocksr/shadowsocksr #
#############################################################

Please enter password for ShadowsocksR:
(Default password: teddysun.com):#########

---------------------------
password = ########
---------------------------

Please enter a port for ShadowsocksR [1-65535]
(Default port: 12619):

---------------------------
port = 12619
---------------------------

Please select stream cipher for ShadowsocksR:
1) none
2) aes-256-cfb
3) aes-192-cfb
4) aes-128-cfb
5) aes-256-cfb8
6) aes-192-cfb8
7) aes-128-cfb8
8) aes-256-ctr
9) aes-192-ctr
10) aes-128-ctr
11) chacha20-ietf
12) chacha20
13) salsa20
14) xchacha20
15) xsalsa20
16) rc4-md5
Which cipher you'd select(Default: aes-256-cfb):

---------------------------
cipher = aes-256-cfb
---------------------------

Please select protocol for ShadowsocksR:
1) origin
2) verify_deflate
3) auth_sha1_v4
4) auth_sha1_v4_compatible
5) auth_aes128_md5
6) auth_aes128_sha1
7) auth_chain_a
8) auth_chain_b
9) auth_chain_c
10) auth_chain_d
11) auth_chain_e
12) auth_chain_f
Which protocol you'd select(Default: origin):

---------------------------
protocol = origin
---------------------------

Please select obfs for ShadowsocksR:
1) plain
2) http_simple
3) http_simple_compatible
4) http_post
5) http_post_compatible
6) tls1.2_ticket_auth
7) tls1.2_ticket_auth_compatible
8) tls1.2_ticket_fastauth
9) tls1.2_ticket_fastauth_compatible
Which obfs you'd select(Default: plain):

---------------------------
obfs = plain
---------------------------

Press any key to start...or Press Ctrl+C to cancel

一键加速VPS服务器
yum -y install wget
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh
chmod +x bbr.sh
./bbr.sh

按任意键继续并重启服务器，服务端到此

安装ShadowsocksR-4.7.0-win客户端即可
输入相关信息即可
注意设置浏览器代理模式
127.0.0.1和1080即可

常用命令
启动：service shadowsocks start
停止：service shadowsocks stop
重启：service shadowsocks restart
状态：service shadowsocks status

windows 客户端下载地址：

<a href="https://github.com/shadowsocksrr/shadowsocksr-csharp/releases">https://github.com/shadowsocksrr/shadowsocksr-csharp/releases</a>
