include $(TOPDIR)/rules.mk

PKG_NAME:=ot-br
PKG_VERSION:=ot-br-1.0
PKG_BUILD_DIR:= $(BUILD_DIR)/$(PKG_NAME)

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
	SECTION:=net
	CATEGORY:=Network
	TITLE:=OpenThread border router config scripts
	SUBMENU:=ot-br
	DEPENDS:= +kmod-usb-acm +kmod-tun +openthread-br +tayga +luci +totd

endef

define Build/Compile
	true
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/etc/thread/
	$(CP) -f ./files/* $(1)
endef


$(eval $(call BuildPackage,$(PKG_NAME)))