# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2021-2022 ImmortalWrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=rtl8188eu
PKG_RELEASE:=1

PKG_SOURCE_URL:=https://github.com/aircrack-ng/rtl8188eus.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2025-02-04
PKG_SOURCE_VERSION:=af3bf004458f76b7aec33e9ba552cd382ed1f5c3
PKG_MIRROR_HASH:=93da00df2229dc178e4d5cc0829313b71311a7cbce5ae6f6fdf66791422daa8f

PKG_BUILD_PARALLEL:=1

STAMP_CONFIGURED_DEPENDS := $(STAGING_DIR)/usr/include/mac80211-backport/backport/autoconf.h

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/rtl8188eu
  SUBMENU:=Wireless Drivers
  TITLE:=Realtek 8188EU(S)/8188ETV support
  DEPENDS:=+kmod-cfg80211 +kmod-usb-core
  FILES:=$(PKG_BUILD_DIR)/rtl8188eu.ko
  AUTOLOAD:=$(call AutoProbe,rtl8188eu)
endef

NOSTDINC_FLAGS:= \
	$(KERNEL_NOSTDINC_FLAGS) \
	-I$(PKG_BUILD_DIR) \
	-I$(PKG_BUILD_DIR)/include \
	-I$(PKG_BUILD_DIR)/platform \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-include backport/autoconf.h \
	-include backport/backport.h

NOSTDINC_FLAGS+= \
	-DRTW_SINGLE_WIPHY \
	-DRTW_USE_CFG80211_STA_EVENT \
	-DCONFIG_IOCTL_CFG80211 \
	-DCONFIG_CONCURRENT_MODE \
	-DBUILD_OPENWRT

ifeq ($(CONFIG_BIG_ENDIAN),y)
NOSTDINC_FLAGS += -DCONFIG_BIG_ENDIAN
else
NOSTDINC_FLAGS += -DCONFIG_LITTLE_ENDIAN
endif

define Build/Compile
	+$(KERNEL_MAKE) $(PKG_JOBS) \
		M="$(PKG_BUILD_DIR)" \
		NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
		CONFIG_RTL8188EU=m \
		USER_MODULE_NAME=rtl8188eu \
		modules
endef

$(eval $(call KernelPackage,rtl8188eu))
