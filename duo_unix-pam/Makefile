#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=duo_unix-pam
PKG_VERSION:=1.11.3
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git://github.com/duosecurity/duo_unix.git
PKG_SOURCE_DATE:=2019-10-10
PKG_MIRROR_HASH:=beabba9fc286fea651941548aa830920f465ca6b754adb6fbc050ad420d3ae75
PKG_SOURCE_VERSION:=52715555339b90a2cd79e50b4acf303192540d45

PKG_MAINTAINER:=Avi <strykar@hotmail.com>
PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYING

PKG_FIXUP:=autoreconf
PKG_MACRO_PATHS:=autotools
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/duo_unix-pam
	SECTION:=admin
	CATEGORY:=Administration
	TITLE:=Duo two-factor authentication
	DEPENDS:= +libopenssl +libpam +openssh-server-pam
	URL:=https://github.com/duosecurity/duo_unix
endef

define Package/duo_unix-pam/description
	Duo Unix with Pluggable Authentication Modules (PAM) support provides a secure and customizable
	method for protecting Unix and Linux logins. Use pam_duo.so with openssh-server-pam and
	/etc/pam.d/sshd See - https://duo.com/docs/duounix
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	# Comment out the X509_TEA_set_state test check in configure.ac See - https://github.com/duosecurity/duo_unix/issues/170
	sed --in-place=.default -e '99s/^/#/' -e '101s/^/#/' -e '102s/^/#/' -e '103s/^/#/' -e '104s/^/#/' -e '105s/^/#/' -e '106s/^/#/' -e '107s/^/#/' $(PKG_BUILD_DIR)/configure.ac
endef

CONFIGURE_VARS += \
         CROSS_COMPILE="yes"

CONFIGURE_ARGS += \
		--with-pam=/lib/security \
		--with-privsep-user=sshd \
		--with-openssl=$(STAGING_DIR)/usr

define Package/duo_unix-pam/install
	$(INSTALL_DIR) $(1)/etc/duo/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/pam_duo/pam_duo.conf $(1)/etc/duo
	
	$(INSTALL_DIR) $(1)/lib/security/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/pam_duo/.libs/pam_duo.so $(1)/lib/security/
endef

$(eval $(call BuildPackage,duo_unix-pam))
