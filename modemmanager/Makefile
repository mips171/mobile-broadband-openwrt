#
# Copyright (C) 2016 Velocloud Inc.
# Copyright (C) 2016 Aleksander Morgado <aleksander@aleksander.es>
#
# This is free software, licensed under the GNU General Public License v2.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=modemmanager
PKG_VERSION:=1.7.991-20180404
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://anongit.freedesktop.org/git/ModemManager/ModemManager.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=7e94928301a971488ea53b17dd7d5ef37f468b1d
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz

PKG_MAINTAINER:=Aleksander Morgado <aleksander@aleksander.es>
PKG_LICENSE:=GPL-2.0+

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/modemmanager
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Control utility for any kind of mobile broadband modem
  URL:=https://www.freedesktop.org/wiki/Software/ModemManager
  DEPENDS:= \
	$(INTL_DEPENDS) \
	+glib2 \
	+libqmi \
	+libmbim \
	+dbus \
	+usb-modeswitch \
	+ppp \
	+kmod-usb-net \
	+kmod-usb-net-cdc-ncm \
	+kmod-usb-net-huawei-cdc-ncm \
	+kmod-usb-net-cdc-mbim \
	+kmod-usb-net-qmi-wwan \
	+kmod-usb-net-hso \
	+kmod-usb-net-rndis \
	+kmod-usb-net-sierrawireless \
	+kmod-usb-acm \
	+kmod-usb-wdm \
	+kmod-usb-serial \
	+kmod-usb-serial-option \
	+kmod-usb-serial-qualcomm \
	+kmod-usb-serial-sierrawireless \
	+kmod-usb-serial-option \
	+kmod-usb-serial-wwan
endef

define Package/modemmanager/description
  ModemManager is a DBus-activated service which allows controlling mobile
  broadband modems.
endef

CONFIGURE_ARGS += \
	--prefix=/usr \
	--sysconfdir=/etc \
	--localstatedir=/var \
	--with-qmi \
	--with-mbim \
	--without-polkit \
	--without-udev \
	--without-suspend-resume \
	--with-systemdsystemunitdir=no \

define Build/Prepare
	$(call Build/Prepare/Default)
	( cd "$(PKG_BUILD_DIR)"; \
		echo -e "all:\ninstall:" >po/Makefile.in.in; \
	)
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/ModemManager
	$(CP) $(PKG_INSTALL_DIR)/usr/include/ModemManager/*.h $(1)/usr/include/ModemManager
	$(INSTALL_DIR) $(1)/usr/include/libmm-glib
	$(CP) $(PKG_INSTALL_DIR)/usr/include/libmm-glib/*.h $(1)/usr/include/libmm-glib
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libmm-glib.so* $(1)/usr/lib
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/ModemManager.pc $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/mm-glib.pc $(1)/usr/lib/pkgconfig
endef

define Package/modemmanager/install
	$(INSTALL_DIR) $(1)/lib/udev/rules.d
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/lib/udev/rules.d/*.rules $(1)/lib/udev/rules.d

	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/ModemManager $(1)/usr/sbin

	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/mmcli $(1)/usr/bin

	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libmm-glib.so* $(1)/usr/lib

	$(INSTALL_DIR) $(1)/usr/lib/ModemManager
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/ModemManager/libmm-plugin-*.so* $(1)/usr/lib/ModemManager

	$(INSTALL_DIR) $(1)/etc/dbus-1/system.d
	$(CP) $(PKG_INSTALL_DIR)/etc/dbus-1/system.d/org.freedesktop.ModemManager1.conf $(1)/etc/dbus-1/system.d

	$(INSTALL_DIR) $(1)/usr/share/dbus-1/system-services
	$(CP) $(PKG_INSTALL_DIR)/usr/share/dbus-1/system-services/org.freedesktop.ModemManager1.service $(1)/usr/share/dbus-1/system-services

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/modemmanager.init $(1)/etc/init.d/modemmanager

	$(INSTALL_DIR) $(1)/etc/hotplug.d/net
	$(INSTALL_DATA) ./files/25-modemmanager-net $(1)/etc/hotplug.d/net

	$(INSTALL_DIR) $(1)/etc/hotplug.d/tty
	$(INSTALL_DATA) ./files/25-modemmanager-tty $(1)/etc/hotplug.d/tty

	$(INSTALL_DIR) $(1)/etc/modemmanager
	$(INSTALL_DATA) ./files/modemmanager.common    $(1)/etc/modemmanager/modemmanager.common

	$(INSTALL_DIR) $(1)/lib/netifd/proto
	$(INSTALL_BIN) ./files/modemmanager.proto $(1)/lib/netifd/proto/modemmanager.sh
endef

$(eval $(call RequireCommand,xsltproc, \
        $(PKG_NAME) requires xsltproc installed on the host-system. \
))

$(eval $(call RequireCommand,intltoolize, \
        $(PKG_NAME) requires intltool installed on the host-system. \
))

$(eval $(call BuildPackage,modemmanager))
