#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
# Modified from https://github.com/mchwalisz/mosh-openwrt/blob/master/mosh/Makefile
#

include $(TOPDIR)/rules.mk

PKG_NAME:=mosh
PKG_VERSION:=1.3.2
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git://github.com/mobile-shell/mosh.git
PKG_MIRROR_HASH:=da600573dfa827d88ce114e0fed30210689381bbdcff543c931e4d6a2e851216
PKG_SOURCE_VERSION:=cf73e1f8799b01ad1ed9731c6b3d239b68509222

PKG_MAINTAINER:=Avi <strykar@hotmail.com>
PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYING

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1
TARGET_LDFLAGS+=-lncurses

include $(INCLUDE_DIR)/package.mk

define Package/mosh/Default
	SECTION:=net
	CATEGORY:=Network
	TITLE:=Mosh mobile shell
	DEPENDS:=+libncurses +libopenssl +protobuf
	URL:=https://github.com/mobile-shell/mosh
endef

define Package/mosh/Default/description
	Mosh is a remote terminal application that allows roaming, supports
	intermittent connectivity, and provides intelligent local echo and line
	editing of user keystrokes.
endef

define Package/mosh-client/description
	$(call Package/mosh/Default/description)
	This is a mosh client.
endef

define Package/mosh-server/description
	$(call Package/mosh/Default/description)
	This is a mosh server.
endef

define Package/mosh-full/description
	$(call Package/mosh/Default/description)
	This is the full package with a perl wrapper script
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	# Comment out checks to verify locale calls for UTF-8. Do not use a patch as this cannot be upstreamed.
	sed --in-place=.default -e '89s,^,//,' -e '90s,^,//,' -e '91s,^,//,' -e '92s,^,//,' $(PKG_BUILD_DIR)/src/util/locale_utils.cc
endef

CONFIGURE_VARS += \
CROSS_COMPILE="yes"

CONFIGURE_ARGS += \
	--with-ncurses=$(STAGING_DIR)/usr \
	--disable-hardening \
	--without-utempter \
	--with-openssl=$(STAGING_DIR)/usr

define Package/mosh-client
	$(call Package/mosh/Default)
	TITLE+= (client)
endef

define Package/mosh-server
	$(call Package/mosh/Default)
	TITLE+= (server)
endef

define Package/mosh-full
	$(call Package/mosh/Default)
	TITLE+= (client)
	DEPENDS:= \
	+mosh-client \
	+mosh-server \
	+perlbase-essential \
	+perlbase-socket \
	+perlbase-xsloader \
	+perlbase-getopt \
	+perlbase-errno \
	+perlbase-config \
	+perlbase-io \
	+perlbase-symbol \
	+perlbase-selectsaver \
	+perlbase-posix \
	+perlbase-autoloader \
	+perlbase-fcntl \
	+perlbase-tie
endef

define Package/mosh-client/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/mosh-client $(1)/usr/bin/
endef

define Package/mosh-server/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/mosh-server $(1)/usr/bin/
endef

define Package/mosh-full/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/mosh $(1)/usr/bin/
endef

$(eval $(call BuildPackage,mosh-client))
$(eval $(call BuildPackage,mosh-server))
$(eval $(call BuildPackage,mosh-full))
