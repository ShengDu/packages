#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=e2guardian
PKG_VERSION:=3.0.4
PKG_RELEASE:=1

PKG_LICENSE:=GPL-2.0
PKG_MAINTAINER:=Luka Perkov <luka@openwrt.org>

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_PROTO:=git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/e2guardian/e2guardian.git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)

PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1

include $(INCLUDE_DIR)/uclibc++.mk
include $(INCLUDE_DIR)/package.mk

define Package/e2guardian
  SECTION:=net
  DEPENDS:=+libpthread $(CXX_DEPENDS) +zlib +libpcre
  CATEGORY:=Network
  SUBMENU:=Web Servers/Proxies
  TITLE:=E2Guardian
  URL:=http://e2guardian.org/cms/
endef

define Package/e2guardian/conffiles
/etc/e2guardian/e2guardianf1.conf
/etc/config/e2guardian
endef

CONFIGURE_VARS += \
	INCLUDES="" \
	CXXFLAGS="$$$$CXXFLAGS -fno-rtti" \
	LIBS="-lpthread"

CONFIGURE_ARGS += \
		--with-sysconfsubdir=e2guardian \
		--with-proxyuser=root \
		--with-proxygroup=root \
		--enable-pcre=yes

define Build/Configure
	$(call Build/Configure/Default,$CONFIGURE_ARGS)
endef

define Package/e2guardian/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/e2guardian $(1)/usr/sbin/

	$(INSTALL_DIR) $(1)/etc
	$(CP) $(PKG_INSTALL_DIR)/etc/e2guardian $(1)/etc/
	$(INSTALL_CONF) ./files/e2guardianf1.conf $(1)/etc/e2guardian/e2guardianf1.conf

	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/e2guardian.config $(1)/etc/config/e2guardian

	$(INSTALL_DIR) $(1)/usr/share/e2guardian
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/share/e2guardian/transparent1x1.gif $(1)/usr/share/e2guardian/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/share/e2guardian/blockedflash.swf $(1)/usr/share/e2guardian/

	$(INSTALL_DIR) $(1)/usr/share/e2guardian/languages/ukenglish
	$(CP) $(PKG_INSTALL_DIR)/usr/share/e2guardian/languages/ukenglish/* $(1)/usr/share/e2guardian/languages/ukenglish/

	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_BIN) ./files/e2guardian.init $(1)/etc/init.d/e2guardian
endef

$(eval $(call BuildPackage,e2guardian))
