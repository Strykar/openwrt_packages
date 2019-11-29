# openwrt_packages
Some OpenWrt package Makefiles I made with help from _#openwrt-devel_.</br> See https://forum.openwrt.org/t/testers-wanted-duo-2-factor-auth-for-ssh-and-mosh/48684

TODO: Create a feed, I currently cross compile for [x86_64](https://openwrt.org/toh/pcengines/apu2) and [MT7628NN](https://openwrt.org/toh/tp-link/tl-mr3020_v3). I do have some [bcm53xx](https://openwrt.org/docs/techref/targets/bcm53xx) and [ath79](https://openwrt.org/docs/techref/targets/ath79) devices.

1. [duo_unix](https://github.com/Strykar/openwrt_packages/tree/master/duo_unix) - Duo Unix is a stand alone executable that can be used to protect programs such as OpenSSH or Sudo. `login_duo` is	built to use with `openssh-server` without PAM support. Configure `sshd_config` with `ForceCommand=/usr/sbin/login_duo`. This will [not work with Mosh](https://github.com/mobile-shell/mosh/issues/506).</br>
Duo offers free 2FA up to 10 users, if you have already setup Duo users and 2FA phones/YubiKeys. this will just work. See - https://duo.com/docs/loginduo

2. [duo_unix-pam](https://github.com/Strykar/openwrt_packages/tree/master/duo_unix-pam) - Duo Unix with Pluggable Authentication Modules (PAM) support provides a secure and customizable method for protecting Unix and Linux logins. `pam_duo.so` is for use with `openssh-server-pam`.</br> See - https://duo.com/docs/duounix

Example `/etc/pam.d/sshd` config:
```
#%PAM-1.0
auth required pam_env.so
auth sufficient pam_duo.so
auth requisite pam_succeed_if.so uid >= 500 quiet
auth required pam_deny.so
account include system-remote-login
password include system-remote-login
session include system-remote-login
```
Example `/etc/ssh/sshd_config` config:
```
PubkeyAuthentication yes
PasswordAuthentication no
UsePAM yes
ChallengeResponseAuthentication yes
UseDNS no
```

3. [mosh](https://github.com/Strykar/openwrt_packages/tree/master/mosh) - Mosh is a UDP replacement for interactive SSH terminals. It's more robust and responsive, especially over Wi-Fi, cellular, and long-distance links burdened with latency. See - https://mosh.org</br> Instructions to build for OpenWrt at https://github.com/mobile-shell/mosh/wiki/Build-Instructions</br>
The Makefile is based off an old version by Entware.
