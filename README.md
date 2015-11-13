
#install dependency libs.
sudo apt-get install build-essential subversion libncurses5-dev zlib1g-dev gawk gcc-multilib flex git-core gettext

#install GCC

sudo apt-get install gcc g++ binutils patch bzip2 flex bison make autoconf gettext texinfo unzip sharutils subversion
libncurses5-dev ncurses-term zlib1g-dev 

#Download source code of OPENWRT.

cd /opt

sudo wget http://downloads.openwrt.org/snapshots/trunk/ar71xx/nand/OpenWrt-ImageBuilder-ar71xx-nand.Linux-x86_64.tar.bz2

sudo tar -xjf OpenWrt-ImageBuilder-ar71xx-nand.Linux-x86_64.tar.bz2

cd OpenWrt-ImageBuilder-ar71xx-nand.Linux-x86_64

make info

#available profile
#WNDR4300:
	#NETGEAR WNDR3700v4/WNDR4300
#	Packages: kmod-usb-core kmod-usb-ohci kmod-usb2 kmod-ledtrig-usbdev



#install openssl library

sudo apt-get install openssl

#WNDR4300完全使用128Mflash：
backup makefile first.

packages :
echo $(wget -qO - http://downloads.openwrt.org/snapshots/trunk/ar71xx/generic/config | sed -ne 's/^CONFIG_PACKAGE_\([a-z0-9-]*\)=y/\1/ip')

base-files busybox dnsmasq-full dropbear firewall fstools jsonfilter libc libgcc mtd netifd opkg procd swconfig ubox ubus ubusd uci usign kmod-crypto-aes kmod-crypto-arc4 kmod-crypto-core kmod-ledtrig-usbdev kmod-lib-crc-ccitt kmod-nls-base kmod-ip6tables kmod-ipt-conntrack kmod-ipt-core kmod-ipt-nat kmod-nf-conntrack kmod-nf-conntrack6 kmod-nf-ipt kmod-nf-ipt6 kmod-nf-nat kmod-nf-nathelper kmod-ipv6 kmod-ppp kmod-pppoe kmod-pppox kmod-slhc kmod-gpio-button-hotplug kmod-usb-core kmod-usb-ohci kmod-usb2 kmod-usb3 kmod-ath kmod-ath9k kmod-ath9k-common kmod-cfg80211 kmod-mac80211 libip4tc libip6tc libxtables libblobmsg-json libiwinfo libjson-c libnl-tiny libubox libubus libuci ip6tables iptables hostapd-common iw odhcp6c odhcpd ppp ppp-mod-pppoe wpad-mini iwinfo jshn libjson-script uboot-envtools luci-ssl Shadowsocks-libev

cp ./openwrt/target/linux/ar71xx/image/Makefile  ./openwrt/target/linux/ar71xx/image/Makefile.bak
sudo vi ./target/linux/ar71xx/image/Makefile
将
wndr4300_mtdlayout=mtdparts=ar934x-nfc:256k(u-boot)ro,256k(u-boot-env)ro,256k(caldata),512k(pot),2048k(language),512k(config),3072k(traffic_meter),2048k(kernel),23552k(ubi),25600k@0x6c0000(firmware),256k(caldata_backup),-(reserved)

改为（将ubi和firmware增加96M，完全使用128M flash）

wndr4300_mtdlayout=mtdparts=ar934x-nfc:256k(u-boot)ro,256k(u-boot-env)ro,256k(caldata),512k(pot),2048k(language),512k(config),3072k(traffic_meter),2048k(kernel),121856k(ubi),123904k@0x6c0000(firmware),256k(caldata_backup),-(reserved)


start to complie.

make image PROFILE=WNDR4300 PACKAGES="base-files busybox dnsmasq-full dropbear firewall fstools jsonfilter libc libgcc mtd netifd opkg procd swconfig ubox ubus ubusd uci usign kmod-crypto-aes kmod-crypto-arc4 kmod-crypto-core kmod-ledtrig-usbdev kmod-lib-crc-ccitt kmod-nls-base kmod-ip6tables kmod-ipt-conntrack kmod-ipt-core kmod-ipt-nat kmod-nf-conntrack kmod-nf-conntrack6 kmod-nf-ipt kmod-nf-ipt6 kmod-nf-nat kmod-nf-nathelper kmod-ipv6 kmod-ppp kmod-pppoe kmod-pppox kmod-slhc kmod-gpio-button-hotplug kmod-usb-core kmod-usb-ohci kmod-usb2 kmod-usb3 kmod-ath kmod-ath9k kmod-ath9k-common kmod-cfg80211 kmod-mac80211 libip4tc libip6tc libxtables libblobmsg-json libiwinfo libjson-c libnl-tiny libubox libubus libuci ip6tables iptables hostapd-common iw odhcp6c odhcpd ppp ppp-mod-pppoe wpad-mini iwinfo jshn libjson-script uboot-envtools luci-ssl" FILES=/opt/openwrt-fanqiang/openwrt-fanqiang/openwrt
