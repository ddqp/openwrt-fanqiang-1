# openwrt-fanqiang
#install dependency libs.
sudo apt-get install build-essential subversion libncurses5-dev zlib1g-dev gawk gcc-multilib flex git-core gettext
#install GCC
sudo apt-get install gcc g++ binutils patch bzip2 flex bison make autoconf gettext texinfo unzip sharutils subversion libncurses5-dev ncurses-term zlib1g-dev 
#Download source code of OPENWRT.
svn checkout svn://svn.openwrt.org/openwrt/trunk 
cd trunk 
./scripts/feeds update -a 
./scripts/feeds install -a 

Add USB support.
打trunk/target/linux/ar71xx/files/arch/mips/ath79
貌似841n v3用mach-tl-wr941nd.cmach-tl-wr841nd.c保险起见我两都换

蛋疼线启atheros网卡需要改package/mac80211/files/lib/wifi/mac80211.shpackage/madwifi/files/lib/wifi/madwifi.sh拉蛋疼

option disable 1 

1改0OK

