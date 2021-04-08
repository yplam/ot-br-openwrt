include $(TOPDIR)/rules.mk

PKG_NAME:=ot-br
PKG_VERSION:=ot-br-1.0
PKG_BUILD_DIR:= $(BUILD_DIR)/$(PKG_NAME)

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
	SECTION:=net
	CATEGORY:=Network
	TITLE:=OpenThread border router config scripts
	DEPENDS:= +kmod-usb-acm +kmod-tun +openthread-br +tayga +luci +totd
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
endef

define Build/Compile
	$(TARGET_CC) $(TARGET_CFLAGS) $(EXTRA_CFLAGS) -Wall -Werror -o $(PKG_BUILD_DIR)/pinmux src/pinmux.c ;
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/pinmux $(1)/sbin/mt7688_pinmux
	$(INSTALL_DIR) $(1)/etc/thread/
	$(CP) -f ./files/* $(1)
endef


$(eval $(call BuildPackage,$(PKG_NAME)))