
include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=hostapd-devel-mtlk-benjaminlevine

PKG_SOURCE_VERSION:=5d981bd
#PKG_SOURCE_DATE:=2017-10-09
#PKG_VERSION:=$(PKG_SOURCE_VERSION).$(PKG_SOURCE_DATE)
PKG_VERSION:=$(PKG_SOURCE_VERSION)
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=https://github.com/benjaminlevine/hostapd-devel-mtlk.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_MIRROR_HASH:=skip

PKG_MAINTAINER:=Jonas Albrecht <plonkbong100@protonmail.com>
PKG_LICENSE:=BSD-3-Clause-Open-MPI
PKG_LICENSE_FILES:=README



include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=WirelessAPD
  TITLE:=experimental hostapd program for wave300 driver
  URL:=https://github.com/benjaminlevine/hostapd-devel-mtlk.git
  DEPENDS:=+libopenssl
  #DEPENDS:=+libopenssl +libopenssl-devcrypto +libssl
  #DEPENDS:=+libopenssl +libopenssl-conf +libopenssl-afalg +libopenssl-devcrypto +libopenssl-padlock +openssl-util
endef

define Package/$(PKG_NAME)/description
	experimental ..
endef

define Build/Configure

  ifeq ("$(wildcard $(PKG_BUILD_DIR)/hostapd/Makefile.bkp)","$(PKG_BUILD_DIR)/hostapd/Makefile.bkp")
		cp -fv $(PKG_BUILD_DIR)/hostapd/Makefile.bkp $(PKG_BUILD_DIR)/hostapd/Makefile
  endif
	sed -i "s:/home/benjamin/router/openwrt:$(BUILD_DIR):" $(PKG_BUILD_DIR)/hostapd/Makefile
	sed -i "s:/.*/toolchain-.*16:$(TOOLCHAIN_DIR):" $(PKG_BUILD_DIR)/hostapd/Makefile
	sed -i "s:/.*/target-.*16:$(TARGET_DIR):" $(PKG_BUILD_DIR)/hostapd/Makefile
	sed -i "s:linux-4.4.14:linux-$(LINUX_VERSION):" $(PKG_BUILD_DIR)/hostapd/Makefile
	$(MAKE) -C $(PKG_BUILD_DIR)/hostapd clean
	
  ifeq ("$(wildcard $(PKG_BUILD_DIR)/wpa_supplicant/Makefile.bkp)","$(PKG_BUILD_DIR)/wpa_supplicant/Makefile.bkp")
	    cp -fv $(PKG_BUILD_DIR)/wpa_supplicant/Makefile.bkp $(PKG_BUILD_DIR)/wpa_supplicant/Makefile
  endif
	sed -i "s:/home/benjamin/router/openwrt:$(BUILD_DIR):" $(PKG_BUILD_DIR)/wpa_supplicant/Makefile
	sed -i "s:/.*/toolchain-.*16:$(TOOLCHAIN_DIR):" $(PKG_BUILD_DIR)/wpa_supplicant/Makefile
	sed -i "s:/.*/target-.*16:$(TARGET_DIR):" $(PKG_BUILD_DIR)/wpa_supplicant/Makefile
	sed -i "s:linux-4.4.14:linux-$(LINUX_VERSION):" $(PKG_BUILD_DIR)/wpa_supplicant/Makefile
	$(MAKE) -C $(PKG_BUILD_DIR)/wpa_supplicant clean
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR)/wpa_supplicant
# the following 4 lines are required because ld fail with symbol not found error
	cp $(PKG_BUILD_DIR)/wpa_supplicant/wpa_cli $(PKG_BUILD_DIR)/hostapd
	cp $(PKG_BUILD_DIR)/wpa_supplicant/wpa_passphrase $(PKG_BUILD_DIR)/hostapd
	cp $(PKG_BUILD_DIR)/wpa_supplicant/wpa_supplicant $(PKG_BUILD_DIR)/hostapd
	$(MAKE) -C $(PKG_BUILD_DIR)/wpa_supplicant clean
	$(MAKE) -C $(PKG_BUILD_DIR)/hostapd
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/hostapd/wpa_supplicant $(1)/usr/sbin/mtlk-wpa_supplicant
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/hostapd/wpa_cli $(1)/usr/sbin/mtlk-wpa_cli
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/hostapd/wpa_passphrase $(1)/usr/sbin/mtlk-wpa_passphrase
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/hostapd/hostapd $(1)/usr/sbin/mtlk-hostapd
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/hostapd/hostapd_cli $(1)/usr/sbin/mtlk-hostapd_cli
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
