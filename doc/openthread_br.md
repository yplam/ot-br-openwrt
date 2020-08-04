OpenThread Border Router 配置
=================================

下面配置一个名称为 MyOT 的网络，允许设备使用 J01NME 密钥加入。

网络相关参数为：

```
panid 0xc80b
extpanid f37fcf5bc5cbe195
```

首先生成对应的 pskc:

```
./pskc J01NME f37fcf5bc5cbe195 MyOT 
# output: 7beafb2ea25f3f8ce244b1e92a79a623
```

Border Router 上使用 ot-ctl 命令配置：

```
ot-ctl panid 0xc80b
ot-ctl extpanid f37fcf5bc5cbe195
ot-ctl masterkey e35efdce91b5d2ad6ee96350c31e56d3
ot-ctl pskc 7beafb2ea25f3f8ce244b1e92a79a623
ot-ctl networkname MyOT
ot-ctl channel 11
ot-ctl ifconfig up
ot-ctl thread start
ot-ctl state

ot-ctl prefix add fd11:22::/64 pasor
ot-ctl netdataregister

```


```
ot-ctl commissioner joiner add f4ce368c8657b05b J01NME

```
如果使用 openwrt，可以直接调用 ubus 接口：

```
ubus call otbr mgmtset '{"masterkey":"e35efdce91b5d2ad6ee96350c31e56d3","networkname":"MyOT","extpanid":"f37fcf5bc5cbe195","panid":"0xc80b","channel":"11","pskc":"7beafb2ea25f3f8ce244b1e92a79a623"}'
ubus call otbr threadstart
ubus call otbr state
```


查看 client 的 eui64

```
> eui64
# f4ce36b1e581f01c
```

使用ubus命令配置 commissioner 

首先，可以使用下面命令查看 otbr 提供的功能

```
ubus -v list otbr
```

```
# 启动 commissioner
ubus call otbr commissionerstart
ubus call otbr joineradd '{"pskd":"J01NME","eui64":"f4ce36b1e581f01c"}'

```

tayga 配置

/etc/config/network

```

config 'interface' 'nat64'
    option 'proto' 'tayga'
    option ipv4_addr 192.168.64.1
    option ipv6_addr 2001:db8:1::1
    option prefix 64:ff9b::/96
    option dynamic_pool 192.168.64.0/24
    option accept_ra 0
    option send_rs 0

```


