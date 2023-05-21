# ot-br-openwrt
This project provide scripts to build a Thread border router on openwrt. 

**Hardware**

* Hi-Link HLK-7688A: MT7688 580MHz MIPS24KEc, 32MB Flash, 128 MB RAM
* nRF52840 RCP: https://openthread.io/codelabs/openthread-hardware#3


**Software**

* Docker development environment
* OpenWrt v22.03.5
* ot-br-posix @c0897a70b907410d9be87eab16c41910bb14a000
* openthread @1203ea6426a906d62a3921b6d4c6a4a6c8e0abde
* ot-nrf528xx @7dc1f1406f437807cdafd6c53a974d76439b6a2b
* totd DNS64


**Initialize development environment**

update git submodules:

```
git submodule update --init
cd /src/ot-br-posix/
git submodule update --init
```

> Note: If you need to use a proxy, uncomment and change the environment configs in docker-compose.yml file .

Install docker and docker-compose, and run:

```
docker-compose up --build
```

To enter your docker development machine, run:

```
docker-compose exec -u docker compiler /bin/bash
```

**Build OpenWrt firmware**

Edit /src/openwrt/feeds.conf, using feeds.conf.default as template:

```
src-git-full packages https://git.openwrt.org/feed/packages.git;openwrt-22.03
src-git-full luci https://git.openwrt.org/project/luci.git;openwrt-22.03
src-git-full routing https://git.openwrt.org/feed/routing.git;openwrt-22.03
src-git-full telephony https://git.openwrt.org/feed/telephony.git;openwrt-22.03
src-link openthread /src/ot-br-posix/etc/openwrt
src-link otbr /src/ot-br
```

Configure and make OpenWrt

```
./scripts/feeds update -a
./scripts/feeds install -a
make menuconfig
# select MediaTek Ralink MIPS -> MT76*8 based boards -> Hi-Link HLK-7688A
# select Network -> ot-br -> ot-br
# select Network -> openthread-br
# select tcpdump if you want to capture and analyze network traffic
make V=99
```
