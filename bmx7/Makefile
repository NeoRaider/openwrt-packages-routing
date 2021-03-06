#    Copyright (C) 2011 Fundacio Privada per a la Xarxa Oberta, Lliure i Neutral guifi.net
#
#    This program is free software; you can redistribute it and/or modify
#    it under the terms of the GNU General Public License as published by
#    the Free Software Foundation; either version 2 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU General Public License for more details.
#
#    You should have received a copy of the GNU General Public License along
#    with this program; if not, write to the Free Software Foundation, Inc.,
#    51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.
#
#    The full GNU General Public License is included in this distribution in
#    the file called "COPYING".
#
# Contibutors:
#	Axel Neumann, Simó Albert i Beltran, Pau Escrich
#


include $(TOPDIR)/rules.mk

PKG_NAME:=bmx7

PKG_SOURCE_PROTO:=git

#PKG_SOURCE_URL:=git://bmx6.net/bmx6.git
PKG_SOURCE_URL:=git://github.com/axn/bmx6.git
#PKG_SOURCE_URL:=file:///home/neumann/bmx6/bmx6.git

PKG_REV:=c893847b458294e4559b8523f3397fbee4b32a8f
PKG_VERSION:=r2015080722
PKG_RELEASE:=4
PKG_LICENSE:=GPL-2.0

PKG_SOURCE_VERSION:=$(PKG_REV)
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE:=$(PKG_SOURCE_SUBDIR).tar.gz
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_SOURCE_SUBDIR)

include $(INCLUDE_DIR)/package.mk

TARGET_CFLAGS += $(FPIC)

MAKE_ARGS += \
        EXTRA_CFLAGS="$(TARGET_CFLAGS) -I. -I$(STAGING_DIR)/usr/include -DCRYPTLIB=POLARSSL_1_3_4 -DCORE_LIMIT=20000 -DTRAFFIC_DUMP -DDEBUG_MALLOC" \
        EXTRA_LDFLAGS="$(TARGET_LDFLAGS) -L$(STAGING_DIR)/usr/lib " \
        GIT_REV="$(PKG_REV)" \
        CC="$(TARGET_CC)" \
        INSTALL_DIR="$(PKG_INSTALL_DIR)" \
        STRIP="/bin/false" \
        build_all

define Package/bmx7/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Routing and Redirection
  TITLE:=BMX7 layer 3 routing daemon
  URL:=http://bmx6.net/
  MAINTAINER:=Axel Neumann <neumann@cgws.de>
  DEPENDS:=+zlib +libpolarssl +kmod-ip6-tunnel +kmod-iptunnel6 +kmod-tun @!bmx6
endef

define Package/bmx7/description
BMX7 layer 3 routing daemon supporting IPv6, and IPv4 over IPv6
endef

define Package/bmx7
  $(call Package/bmx7/Default)
  MENU:=1
endef

define Package/bmx7-uci-config
  $(call Package/bmx7/Default)
  DEPENDS:=bmx7 +libuci
  TITLE:=configuration plugin based on uci (recommended!)
endef

define Package/bmx7-json
  $(call Package/bmx7/Default)
  DEPENDS:=bmx7 +libjson-c
  TITLE:=json plugin based on json-c
endef

define Package/bmx7-sms
  $(call Package/bmx7/Default)
  DEPENDS:=bmx7
  TITLE:=sms plugin
endef

#define Package/bmx7-quagga
#  $(call Package/bmx7/Default)
#  DEPENDS:=bmx7 +qmp-quagga @BROKEN
#  TITLE:=bmx7 quagga plugin to redistribute/export routes (needs manet/bmx patched quagga 0.99.21)
#endef

define Package/bmx7-table
  $(call Package/bmx7/Default)
  DEPENDS:=bmx7
  TITLE:=bmx7 table plugin to automatically announce routing-table routes via ip6ip tunnels
endef

define Build/Configure
	mkdir -p $(PKG_INSTALL_DIR)
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) $(MAKE_ARGS)
endef

define Package/bmx7/install
	$(INSTALL_DIR) $(1)/usr/sbin $(1)/etc/config $(1)/etc/init.d
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bmx6 $(1)/usr/sbin/bmx6
endef

define Package/bmx7-uci-config/conffiles
/etc/config/bmx6
endef

define Package/bmx7-uci-config/install
	$(INSTALL_DIR) $(1)/usr/lib $(1)/etc/config $(1)/etc/init.d
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/bmx6_uci_config/bmx6_config.so $(1)/usr/lib/bmx6_config.so
	$(INSTALL_BIN) ./files/etc/init.d/bmx6 $(1)/etc/init.d/bmx6
	$(INSTALL_DATA) ./files/etc/config/bmx6 $(1)/etc/config/bmx6
endef

define Package/bmx7-json/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/bmx6_json/bmx6_json.so $(1)/usr/lib/bmx6_json.so
endef

define Package/bmx7-sms/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/bmx6_sms/bmx6_sms.so $(1)/usr/lib/bmx6_sms.so
endef

define Package/bmx7-table/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/bmx6_table/bmx6_table.so $(1)/usr/lib/bmx6_table.so
endef

#define Package/bmx7-quagga/install
#	$(INSTALL_DIR) $(1)/usr/lib
#	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/bmx6_quagga/bmx6_quagga.so $(1)/usr/lib/bmx6_quagga.so
#endef

$(eval $(call BuildPackage,bmx7))
$(eval $(call BuildPackage,bmx7-uci-config))
$(eval $(call BuildPackage,bmx7-json))
$(eval $(call BuildPackage,bmx7-sms))
#$(eval $(call BuildPackage,bmx7-quagga))
$(eval $(call BuildPackage,bmx7-table))
