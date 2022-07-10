#
# Copyright (C) 2017 - 2018 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=asterisk-chan-quectel

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/shalzz/asterisk-chan-quectel.git
PKG_SOURCE_VERSION:=dc50e13c4d883df4d949d1ccf4e31b6b2ffd1a31
PKG_SOURCE_DATE=2022-07-10
PKG_RELEASE:=1
PKG_MIRROR_HASH:=468f225d1ff2b80b2a33966f784cf2985b7774eb140add0a1b914d191b6ce50c

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
  URL:=https://github.com/IchthysMaranatha/asterisk-chan-quectel
  DEPENDS:=asterisk $(ICONV_DEPENDS) +kmod-usb-acm +kmod-usb-serial +kmod-usb-serial-option +libusb-1.0 +usb-modeswitch +alsa-lib
  TITLE:= Asterisk Mobile Telephony Module for quectel chips
endef

define Package/asterisk-chan-quectel/description
 Asterisk channel driver for quectel chips.
endef

CONFIGURE_ARGS+= \
	--with-asterisk=$(STAGING_DIR)/usr/include \
	--with-astversion=18 \
	--with-iconv=$(ICONV_PREFIX)/include \
	--enable-debug
#	DESTDIR=/usr/lib/asterisk/modules

TARGET_CFLAGS+= \
	-I$(CHAN_quectel_AST_HEADERS)

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
