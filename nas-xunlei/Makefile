#
# Copyright (C) 2015-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v3.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=nas-xunlei
PKG_VERSION:=3.5.2-2
PKG_RELEASE:=4
STRIP:=false

ifeq ($(ARCH),aarch64)
	BIN_HASH:=b65e56ca4749d0fab18f2a1c0d37edec9377205fe3d94033c2ac316557ea4f10
endif

ifeq ($(ARCH),x86_64)
	BIN_HASH:=8ecb2329c9a318d37e5c8a59bf9cfd71138df7e3fab91c888f0e7a30931bebaa
endif

PKG_MAINTAINER:=sbwml <admin@cooluc.com>
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Web Servers/Proxies
  TITLE:=nas-xunlei
  DEPENDS:=@(aarch64||x86_64) @USE_MUSL +libc6-compat +libgcc +libstdcpp
  URL:=https://nas.xunlei.com/
endef

define Package/$(PKG_NAME)/description
  NAS Thunder download service
endef

define Package/$(PKG_NAME)/postrm
#!/bin/sh
  rm -rf /usr/share/xunlei
  exit 0
endef

define Download/$(PKG_NAME)
  URL:=https://github.com/sbwml/nas-xunlei-binary/releases/download/v$(PKG_VERSION)
  URL_FILE:=$(ARCH)-v$(PKG_VERSION).tar.xz
  FILE:=nas-xunlei-$(ARCH)-v$(PKG_VERSION).tar.xz
  HASH:=$(BIN_HASH)
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	$(eval $(call Download,$(PKG_NAME)))
	$(TAR) -xf $(DL_DIR)/nas-xunlei-$(ARCH)-v$(PKG_VERSION).tar.xz -C $(PKG_BUILD_DIR)/
endef

define Build/Compile
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/share/xunlei
	$(CP) $(PKG_BUILD_DIR)/bin $(1)/usr/share/xunlei/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/xunlei $(1)/usr/share/xunlei/xunlei

	$(INSTALL_DIR) $(1)/usr/syno/synoman/webman/modules $(1)/etc
	$(INSTALL_CONF) $(CURDIR)/files/synoinfo.conf $(1)/etc/synoinfo.conf
	$(INSTALL_BIN) $(CURDIR)/files/authenticate.cgi $(1)/usr/syno/synoman/webman/modules/authenticate.cgi

	$(INSTALL_DIR) $(1)/usr/share/xunlei/hack
	$(CP) $(PKG_BUILD_DIR)/hack/libc.so.6 $(1)/usr/share/xunlei/hack/
	$(CP) $(PKG_BUILD_DIR)/hack/libdl.so.2 $(1)/usr/share/xunlei/hack/
	$(CP) $(PKG_BUILD_DIR)/hack/libm.so.6 $(1)/usr/share/xunlei/hack/
	$(CP) $(PKG_BUILD_DIR)/hack/libpthread.so.0 $(1)/usr/share/xunlei/hack/

endef

$(eval $(call BuildPackage,$(PKG_NAME)))