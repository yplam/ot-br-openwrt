include $(TOPDIR)/rules.mk

PKG_NAME:=mqttsn_gateway
PKG_VERSION:=master
PKG_BUILD_DIR:= $(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/eclipse/paho.mqtt-sn.embedded-c.git
PKG_SOURCE_DATE:=2020-07-09
PKG_SOURCE_VERSION:=ab94a094b543b1b24c28ab58d741ffb928cec48f
PKG_HASH:=e920037fe1658cc545d400e2c3139e7488f33be3745f6d6f7dc1f56a1cadcef2

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
	SECTION:=net
	CATEGORY:=Network
	TITLE:=mqttsn_gateway
	DEPENDS:=+libc +libopenssl +librt +libstdcpp +libpthread
endef

define Package/$(PKG_NAME)/description
	mqttsn_gateway
endef


define Build/Prepare
	$(Build/Prepare/Default)
endef

# uses GNU configure

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR)/MQTTSNGateway SENSORNET=udp6 \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		CXX="$(TARGET_CXX)" \
		Build/MQTT-SNGateway
endef

define Package/mqttsn_gateway/install
	$(INSTALL_DIR) $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/MQTTSNGateway/Build/MQTT-SNGateway $(1)/usr/sbin/
	$(CP) -f ./files/* $(1)
endef

$(eval $(call BuildPackage,mqttsn_gateway))
