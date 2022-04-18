#
# Copyright (C) 2006-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=wave300-fw-spw921v

PKG_SOURCE_VERSION:=898037e
#PKG_SOURCE_DATE:=2021-11-21
#PKG_VERSION:=$(PKG_SOURCE_VERSION).$(PKG_SOURCE_DATE)
PKG_VERSION:=$(PKG_SOURCE_VERSION)
PKG_RELEASE:=2

PKG_SOURCE_PROTO:=git
PKG_SOURCE_SUBDIR:=wave300-garlett-$(PKG_VERSION)
PKG_SOURCE:=$(PKG_SOURCE_SUBDIR).tar.xz
PKG_SOURCE_URL:=https://github.com/garlett/wave300.git
PKG_MIRROR_HASH:=skip

PKG_MAINTAINER:=Jonas Albrecht <plonkbong100@protonmail.com>

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
  SECTION:=kernel
  CATEGORY:=Kernel modules
  SUBMENU:=Wireless Drivers
  TITLE:=wave300 binary firmware for experimental driver from garlett
  URL:=https://github.com/garlett/wave300.git
  DEPENDS:=+wireless-tools
  PKGFLAGS:=shared
  LICENSE:=UKNOWN
  LICENSE_FILES:=scripts/firmware/License.txt
endef

define Package/$(PKG_NAME)/description
	experimental ..
endef

FW1_NAME:=D6100
FW1_SOURCE_VERSION:=V1.0.0.60_0.0.60
FW1_VERSION:=$(FW1_SOURCE_VERSION)
FW1_SOURCE_URL:=https://www.downloads.netgear.com/files/GPL/
FW1_SOURCE_HASH:=skip
FW1_SOURCE_SUBDIR:=$(FW1_NAME)-$(FW1_VERSION)_src_GPL
FW1_SOURCE:=$(FW1_NAME)-$(FW1_VERSION)_src.zip
define Download/fw1
	PROTO:=
	URL:=$(FW1_SOURCE_URL)
	FILE:=$(FW1_SOURCE)
	HASH:=skip
endef
$(eval $(call Download,fw1))

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	tar -C $(PKG_BUILD_DIR) -xf $(DL_DIR)/$(PKG_SOURCE)
	( cd $(PKG_BUILD_DIR); unzip $(DL_DIR)/$(FW1_SOURCE); )
endef

define Build/Compile
endef


define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/lib/firmware/wave300fwset-spw921v/
	
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/$(FW1_SOURCE_SUBDIR)/package/lantiq-wave300/files/root/mtlk/images/progmodels/wave300/ProgModel_A_CB_wave300.bin $(1)/lib/firmware/wave300fwset-spw921v/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/$(FW1_SOURCE_SUBDIR)/package/lantiq-wave300/files/root/mtlk/images/progmodels/wave300/ProgModel_A_CB_3A_RevC_wave300.bin $(1)/lib/firmware/wave300fwset-spw921v/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/$(FW1_SOURCE_SUBDIR)/package/lantiq-wave300/files/root/mtlk/images/progmodels/wave300/ProgModel_A_nCB_wave300.bin $(1)/lib/firmware/wave300fwset-spw921v/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/$(FW1_SOURCE_SUBDIR)/package/lantiq-wave300/files/root/mtlk/images/progmodels/wave300/ProgModel_A_nCB_3A_RevC_wave300.bin $(1)/lib/firmware/wave300fwset-spw921v/
	
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/$(FW1_SOURCE_SUBDIR)/package/lantiq-wave300/files/root/mtlk/images/progmodels/wave300/ProgModel_BG_CB_wave300.bin $(1)/lib/firmware/wave300fwset-spw921v/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/$(FW1_SOURCE_SUBDIR)/package/lantiq-wave300/files/root/mtlk/images/progmodels/wave300/ProgModel_BG_CB_3A_RevC_wave300.bin $(1)/lib/firmware/wave300fwset-spw921v/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/$(FW1_SOURCE_SUBDIR)/package/lantiq-wave300/files/root/mtlk/images/progmodels/wave300/ProgModel_BG_nCB_wave300.bin $(1)/lib/firmware/wave300fwset-spw921v/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/$(FW1_SOURCE_SUBDIR)/package/lantiq-wave300/files/root/mtlk/images/progmodels/wave300/ProgModel_BG_nCB_3A_RevC_wave300.bin $(1)/lib/firmware/wave300fwset-spw921v/
	
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/$(PKG_SOURCE_SUBDIR)/scripts/firmware/ahmar/contr_lm.bin $(1)/lib/firmware/wave300fwset-spw921v/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/$(PKG_SOURCE_SUBDIR)/scripts/firmware/ahmar/ap_upper_wave300.bin $(1)/lib/firmware/wave300fwset-spw921v/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/wave300-spw921.init $(1)/etc/init.d/wave300-spw921v
endef

$(eval $(call BuildPackage,$(PKG_NAME))) 
