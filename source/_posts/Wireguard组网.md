---
title: Wireguard组网
date: 2018-06-11 21:52:20
tags: 网络
categories: 网络
thumbnail: http://oowki3u7j.bkt.clouddn.com/wireguardLogo.png
---
# Wireguard组网

## WireGuard
WireGuard是一个新型的VPN，简单快速。现在这个只能部署在linux内核的计算上。[官网查看详情](https://www.wireguard.com/)。

## 网络环境说明
内网中有主机是10.x.x.x的电脑， 然后有一台外网固定ip的国外主机。使用WireGuard来进行联通。

## WireGuard安装
官网有详细的介绍[https://www.wireguard.com/install/](https://www.wireguard.com/install/)。我下面写一下ubuntu伤的安装方法。
```bash
$ sudo add-apt-repository ppa:wireguard/wireguard
$ sudo apt-get update
$ sudo apt-get install wireguard
```

## 配置过程
### 安装
要保证内网机器，和公网的机器都安装好wireguard。

### 配置Ip转发
在公网的机器需要充当网关的作用所以需要配置Ip转发，默认ubuntu是关闭的。
```
sudo vi /etc/sysctl.conf
添加：
net.ipv4.ip_forward=1
运行：
sudo sysctl -p
```
### 生成密钥
在内网机器和公网机器上都需要生成秘要。
```
wg genkey | tee privatekey | wg pubkey > publickey

#查看私钥
cat privatekey
#查看公钥
cat publickey
```

### 配置文件设置
我们在内网机器和公网机器都分别创建一个/etc/wireguard/wg0.conf
#### 公网机器配置
 这里我把公网机器的ip配置成192.168.2.1。里面的PostUp，和PostDown分别来配置iptables转发规则。里面的ens3根据查看自己的ifconfig来修改。
```BASH
[Interface]
Address = 192.168.2.1/24
SaveConfig = true
PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o ens3 -j MASQUERADE
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o ens3 -j MASQUERADE
ListenPort = 23334    #这里是公网机器打开的监听端口
PrivateKey = xxxxxxxxxxxxxx(用自己的公网机器上生成的私钥来替换)

[Peer]
PublicKey = xxxxxxxxxxxxxxx（用内网机器的公钥来替换）
AllowedIPs = 192.168.2.2/32 #把内网机的ip设置为192.168.2.2
Endpoint = 218.75.123.186:39055 # 者个部分由于内网机器没有公网IP可以百度看下自己的ip地址贴上，端口号可以先随意写，等内网机器连接上后端口会自动更新。
```
#### 内网机器配置

```
[Interface]
PrivateKey = xxxxxxxxxxxxxx (用内网机器的私钥)
Address = 192.168.2.2/24    (和上面公网机器配置的保持一直)


[Peer]
PublicKey = xxxxxxxxxxxxxx (填写公网主机的公钥)
Endpoint = x.x.x.x:23334   (x.x.x.x用你的公网ip来填写，后面的公网机器的监听端口号)
AllowedIPs = 0.0.0.0/0, 192.168.2.1/32 （表示允许什么ip地址访问你的wireguard，前面的0.0.0.0/0很重要不添加的会不能访问一直显示网关不可达）
```

### 启动WireGuard
wireGuard quick start后会默认所有的流量都走wg0
```
sudo wg-quick up wg0
```

配置路由问题
```
# ip route del default  #删除默认网关
# ip route add default dev wg0  #设置wg0为默认网关
# ip route add xx.xx.xx.xx/32 via yy.yy.yy.yy dev eth0   #把你公网ip的地址替换xx.xx.xx.xx ,用你的原来的网关替换yy.yy.yy.yy
```

### 配置路由表
中国Ip走国内路线，外国Ip走国外路线，在github上找到一个项目[地址](https://github.com/greensea/cnips) git地址https://github.com/greensea/cnips.git。
```
gcc cnips.c -O2 -o cnips

wget http://ftp.apnic.net/apnic/stats/apnic/delegated-apnic-latest
cat delegated-apnic-latest | ./cnips
```


写一个bash脚本
```
for net in `cat delegated-apnic-latest | ./cnips`
do
         route add -net $net gw 10.1.70.254  #这里的10.1.70.254用原来的网关进行替换
done
```

### 配置DNS
配置好后还发现有问题，类似百度，B站等都有海外服务器用海外的DNS解析出来后都是海外地址，访问很慢。所以还需要自己搭建一个DNS。可以用docker现成项目来做[地址](https://github.com/jpillora/docker-dnsmasq)接下来还有国内常用域名git[地址](https://github.com/zizhengwu/GFW_DNS_SMART_RESOLVE)里面的dnsmasq.conf文件复制到Dns的配置里。






## 参考
wireguard搭建第二套 https://www.sooele.com/index.php/2017/12/14/wireguard%E6%90%AD%E5%BB%BA%E7%AC%AC%E4%BA%8C%E5%A5%97/
