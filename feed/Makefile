#
# Copyright (C) 2017 - 2018 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=asterisk-chan-quectel

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/wdoekes/asterisk-chan-quectel.git
PKG_SOURCE_VERSION:=3d046f7d6842298c6838b5ce5b51d495d383b158
PKG_SOURCE_DATE=2021-10-06
PKG_RELEASE:=1
PKG_MIRROR_HASH:=d485c89a7230ab8c318eed6c3a954b154d7e53cc7a0194abf96f4dcb83e6909c

PKG_FIXUP:=autoreconf

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYRIGHT.txt LICENSE.txt
PKG_MAINTAINER:=Jiri Slachta <jiri@slachta.eu>

MODULES_DIR:=/usr/lib/asterisk/modules

include $(INCLUDE_DIR)/package.mk
# asterisk-chan-quectel needs iconv
include $(INCLUDE_DIR)/nls.mk

define Package/asterisk-chan-quectel
  SUBMENU:=Telephony
  SECTION:=net
  CATEGORY:=Network
  URL:=https://github.com/wdoekes/asterisk-chan-quectel
  DEPENDS:=asterisk $(ICONV_DEPENDS) +libsqlite3
  TITLE:= Asterisk Mobile Telephony Module for quectel chips
endef

define Package/asterisk-chan-quectel/description
 Asterisk channel driver for Huawei UMTS 3G quectel.
endef

CONFIGURE_ARGS+= \
	--with-asterisk=$(STAGING_DIR)/usr/include \
	--with-astversion=18 \
	--with-iconv=$(ICONV_PREFIX)/include

MAKE_FLAGS+=LD="$(TARGET_CC)"

CONFIGURE_VARS += \
	DESTDIR="$(MODULES_DIR)" \
	ac_cv_type_size_t=yes \
	ac_cv_type_ssize_t=yes

define Package/asterisk-chan-quectel/conffiles
/etc/asterisk/quectel.conf
endef

define Package/asterisk-chan-quectel/install
	$(INSTALL_DIR) $(1)/etc/asterisk
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/etc/quectel.conf $(1)/etc/asterisk
	$(INSTALL_DIR) $(1)$(MODULES_DIR)
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/chan_quectel.so $(1)$(MODULES_DIR)
endef

define Package/asterisk-chan-quectel/postinst
#!/bin/sh
if [ -z "$${IPKG_INSTROOT}" ]; then
  echo
  echo "o-------------------------------------------------------------------o"
  echo "| asterisk-chan-quectel note                                         |"
  echo "o-------------------------------------------------------------------o"
  echo "| Adding the \"asterisk\" user to the \"dialout\" group might be        |"
  echo "| required for asterisk to be able to access the quectel.            |"
  echo "o-------------------------------------------------------------=^_^=-o"
  echo
fi
exit 0
endef

$(eval $(call BuildPackage,asterisk-chan-quectel))
