
include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=wave300-garlett

PKG_SOURCE_VERSION:=898037e
#PKG_SOURCE_DATE:=2021-11-21
#PKG_VERSION:=$(PKG_SOURCE_VERSION).$(PKG_SOURCE_DATE)
PKG_VERSION:=$(PKG_SOURCE_VERSION)
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=https://github.com/garlett/wave300.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_MIRROR_HASH:=skip

PKG_MAINTAINER:=Jonas Albrecht <plonkbong100@protonmail.com>



include $(INCLUDE_DIR)/package.mk



define Package/$(PKG_NAME)/Default
  SUBMENU:=Wireless Drivers
  URL:=https://github.com/garlett/wave300.git
endef

define KernelPackage/$(PKG_NAME)
	$(call Package/$(PKG_NAME)/Default)
  TITLE:=experimental wave300 wlan driver from garlett
  DEPENDS:=+libnl
  FILES:=$(PKG_BUILD_DIR)/builds/ugw5.4-vrx288/wireless/driver/linux/mtlk.ko $(PKG_BUILD_DIR)/builds/ugw5.4-vrx288/tools/mtlkroot/linux/mtlkroot.ko
  PKGFLAGS:=shared
  LICENSE:=BSD-2-Clause
  LICENSE_FILES:=LICENSE
endef

define Package/wave300-fw-garlett
	$(call Package/$(PKG_NAME)/Default)
  SECTION:=kernel
  CATEGORY:=Kernel modules
  TITLE:=wave300 binary firmware for experimental driver from garlett
  PKGFLAGS:=shared
  LICENSE:=UKNOWN
  LICENSE_FILES:=scripts/firmware/License.txt
endef



define KernelPackage/$(PKG_NAME)/description
	experimental wlan driver from github.com/garlett for lantiq wave300 wlan driver
endef

define Package/wave300-fw-garlett/description
	firmware for experimental lantiq wave300 wlan driver from github.com/garlett
endef

RFLIB_NAME:=wave300_rflib
RFLIB_SOURCE_URL:=https://repo.or.cz/wave300_rflib.git
#RFLIB_SOURCE_DATE:=2021-11-21
RFLIB_SOURCE_VERSION:=7b250b5
RFLIB_VERSION:=$(RFLIB_SOURCE_VERSION)
RFLIB_SOURCE_HASH:=skip
RFLIB_SOURCE_SUBDIR:=$(RFLIB_NAME)-$(RFLIB_VERSION)
RFLIB_SOURCE:=$(RFLIB_SOURCE_SUBDIR).tar.xz
define Download/rflib
	PROTO:=git
	URL:=$(RFLIB_SOURCE_URL)
	FILE:=$(RFLIB_SOURCE)
	VERSION:=$(RFLIB_SOURCE_VERSION)
	# DATE:=$(RFLIB_SOURCE_DATE)
	SUBDIR:=$(RFLIB_SOURCE_SUBDIR)
endef
$(eval $(call Download,rflib))

define Build/Prepare
	$(call Build/Prepare/Default)
	tar -C $(PKG_BUILD_DIR) -xf $(DL_DIR)/$(RFLIB_SOURCE)
	# $(LN) ../../$(RFLIB_SOURCE_SUBDIR)/wireless/driver/rflib $(PKG_BUILD_DIR)/wireless/driver/rflib
	$(CP) -av $(PKG_BUILD_DIR)/$(RFLIB_SOURCE_SUBDIR)/wireless/driver/rflib $(PKG_BUILD_DIR)/wireless/driver
	$(SED) s:"DEFAULT_TOOLCHAIN_PATH=.*":"DEFAULT_TOOLCHAIN_PATH=\"$(TOPDIR)\"": $(PKG_BUILD_DIR)/support/ugw.env.common
	$(MAKE) -C $(PKG_BUILD_DIR) distclean
	exec echo -ne "dnl Definition of the branch version for configure.ac\nm4_define([MTLK_BRANCH_VERSION], [@BRANCH_VERSION@])" >$(PKG_BUILD_DIR)/branch_version.m4.in
	mkdir -p "$(PKG_BUILD_DIR)/tools/mtidl/.deps/" "$(PKG_BUILD_DIR)/tools/rtlogger/shared/.deps/" "$(PKG_BUILD_DIR)/tools/shared/.deps/"
	touch "$(PKG_BUILD_DIR)/tools/mtidl/.deps/mtidl_ini_parser.Po" "$(PKG_BUILD_DIR)/tools/rtlogger/shared/.deps/logmacro_database.Po" "$(PKG_BUILD_DIR)/tools/shared/.deps/Debug.Po" "$(PKG_BUILD_DIR)/tools/shared/.deps/CmdLine.Po" "$(PKG_BUILD_DIR)/tools/shared/.deps/aux_utils.Po" "$(PKG_BUILD_DIR)/tools/shared/.deps/ParamInfo.Po" "$(PKG_BUILD_DIR)/tools/shared/.deps/argv_parser.Po" "$(PKG_BUILD_DIR)/tools/shared/.deps/mtlk_socket.Po" "$(PKG_BUILD_DIR)/tools/shared/.deps/mtlkirbhash.Po" "$(PKG_BUILD_DIR)/tools/shared/.deps/mtlkcontainer.Po" "$(PKG_BUILD_DIR)/tools/shared/.deps/mtlk_pathutils.Po"
endef


define Build/Configure
	$(MAKE) -C $(PKG_BUILD_DIR) MINICONFIG="--olddefconfig" defconfig
# if you want to configure your build remove the following #
	#$(MAKE) -C $(PKG_BUILD_DIR)/wave300 menuconfig
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR)
endef

define Package/wave300-fw-garlett/install
	$(INSTALL_DIR) $(1)/lib
	$(CP) -a $(PKG_BUILD_DIR)/scripts/firmware $(1)/lib/
endef

$(eval $(call KernelPackage,$(PKG_NAME)))
$(eval $(call BuildPackage,wave300-fw-garlett))
