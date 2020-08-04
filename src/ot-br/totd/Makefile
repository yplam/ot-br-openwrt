include $(TOPDIR)/rules.mk

PKG_NAME:=totd
PKG_VERSION:=1.5.1
PKG_BUILD_DIR:= $(BUILD_DIR)/$(PKG_NAME)

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
	SECTION:=net
	CATEGORY:=Network
	TITLE:=Small DNS proxy that supports IPv6/IPv4 record translation
	URL:=http://www.dillema.net/software/totd.html
endef

define Package/$(PKG_NAME)/description
	totd is a small DNS proxy nameserver which supports IPv6 and enable IPv6
	only sites to access IPv4 sites by using some translation mechanism such
	as NAT-PT, KAME faith, etc...
endef

define Package/totd/conffiles
	/etc/totd.conf
endef


define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	$(CP) ./src/* $(PKG_BUILD_DIR)/
endef

# uses GNU configure

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		CC="$(TARGET_CC)" \
		all
endef

define Package/totd/install
	$(INSTALL_DIR) $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/$(PKG_NAME) $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/etc/
	$(INSTALL_CONF) ./files/totd.conf $(1)/etc/
	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_BIN) ./files/totd.init $(1)/etc/init.d/totd
endef

$(eval $(call BuildPackage,totd))
