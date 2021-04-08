# ot-br-openwrt
This project provide scripts to build a Thread border router on openwrt. 

**Hardware**

* MT7688 580MHz MIPS24KEc, 32MB Flash, 128 MB RAM
* nRF52840 NCP


**Software**

* Docker development environment
* OpenThread NCP firmware @add99687fdb55f917630b895e019a448cf9aad01
* OpenWrt firmware
* ot-br-posix @24b65157eaccf4902edcab6cb1a4f9e60d998947
* tayga NAT64
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

**Build NCP firmware**

Reference: 
 * [OpenThread on nRF52840 Example](https://github.com/openthread/openthread/tree/master/examples/platforms/nrf528xx/nrf52840)
 * [Pre-Built NCP Firmware](https://openthread.io/platforms/co-processor/firmware)

```
cd /src/openthread/
./bootstrap
make -f examples/Makefile-nrf52840 BORDER_AGENT=1 BORDER_ROUTER=1 COMMISSIONER=1 UDP_FORWARD=1 USB=1 LINK_RAW=1 BOOTLOADER=USB
cd /src/openthread/output/nrf52840/bin
arm-none-eabi-objcopy -O ihex ot-rcp ot-rcp.hex
```

Flash ot-rcp.hex to your nrf52840 board.

**Build OpenWrt firmware**

Edit /src/openwrt/feeds.conf, using feeds.conf.default as template:

```
src-git packages https://git.openwrt.org/feed/packages.git;openwrt-19.07
src-git luci https://git.openwrt.org/project/luci.git;openwrt-19.07
src-git routing https://git.openwrt.org/feed/routing.git;openwrt-19.07
src-git telephony https://git.openwrt.org/feed/telephony.git;openwrt-19.07
src-link openthread /src/ot-br-posix/etc/openwrt
src-link otbr /src/ot-br
```

Configure and make OpenWrt

```
./scripts/feeds update -a
./scripts/feeds install -a
make menuconfig
# select MediaTek Ralink MIPS -> MT76*8 based boards -> MediaTek LinkIt Smart 7688
# select Network -> ot-br -> ot-br
# select tcpdump if you want to capture and analyze network traffic
make V=99
```

Change setting path:

```
# /src/ot-br-posix/third_party/openthread/CMakeLists.txt
list(APPEND OT_PLATFORM_DEFINES "-DOPENTHREAD_CONFIG_POSIX_SETTINGS_PATH=\"/etc/thread\"")

```