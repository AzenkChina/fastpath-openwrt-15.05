#
# Copyright (C) 2008-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=shortcut-fe
PKG_RELEASE:=1

include $(INCLUDE_DIR)/package.mk

define KernelPackage/shortcut-fe
	SUBMENU:=Network Support
	TITLE:=Kernel driver for SFE
	DEPENDS:=+kmod-ipv6
	FILES:=$(PKG_BUILD_DIR)/shortcut-fe.ko $(PKG_BUILD_DIR)/shortcut-fe-ipv6.ko
	AUTOLOAD:=$(call AutoLoad,80,shortcut-fe shortcut-fe-ipv6,1)
	KCONFIG:=CONFIG_NF_CONNTRACK_EVENTS=y CONFIG_NF_CONNTRACK_MARK=y
endef

define KernelPackage/shortcut-fe/Description
Shortcut is an in-Linux-kernel IP packet forwarding engine.
endef



define KernelPackage/fast-classifier
	SUBMENU:=Network Support
	TITLE:=Kernel driver for SFE
	DEPENDS:=+kmod-ipt-conntrack +kmod-shortcut-fe
	FILES:=$(PKG_BUILD_DIR)/fast-classifier.ko
	KCONFIG:=CONFIG_NF_CONNTRACK_CHAIN_EVENTS=y
endef

define KernelPackage/fast-classifier/Description
Simple connection manager for the Shortcut forwarding engine.
endef

EXTRA_CFLAGS+=-DSFE_SUPPORT_IPV6


define Build/Prepare
		mkdir -p $(PKG_BUILD_DIR)
		$(CP) ./src/* $(PKG_BUILD_DIR)/
endef

define Build/Compile
	$(MAKE) -C "$(LINUX_DIR)" \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		ARCH="$(LINUX_KARCH)" \
		SUBDIRS="$(PKG_BUILD_DIR)" \
		EXTRA_CFLAGS="$(EXTRA_CFLAGS)" \
		SFE_SUPPORT_IPV6=1 \
		modules
endef

define KernelPackage/shortcut-fe/install
		$(INSTALL_DIR) $(1)/etc/init.d
		$(INSTALL_BIN) ./files/etc/init.d/fast-classifier $(1)/etc/init.d/fast-classifier
endef

$(eval $(call KernelPackage,shortcut-fe))
$(eval $(call KernelPackage,fast-classifier))
