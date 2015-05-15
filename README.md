
#install dependency libs.
sudo apt-get install build-essential subversion libncurses5-dev zlib1g-dev gawk gcc-multilib flex git-core gettext
#install GCC
sudo apt-get install gcc g++ binutils patch bzip2 flex bison make autoconf gettext texinfo unzip sharutils subversion libncurses5-dev ncurses-term zlib1g-dev 
#Download source code of OPENWRT.

cd /opt
sudo git clone https://github.com/openwrt-mirror/openwrt

download source code of shadowsocks.
cd /opt/openwrt/package
sudo git clone https://github.com/madeye/shadowsocks-libev.git

cd /opt/openwrt
sudo ./scripts/feeds update -a 
sudo ./scripts/feeds install -a 
sudo make defconfig

#install openssl library
sudo apt-get install openssl
#WNDR4300完全使用128Mflash：
backup makefile first.

cp ./openwrt/target/linux/ar71xx/image/Makefile  ./openwrt/target/linux/ar71xx/image/Makefile.bak
sudo vi ./target/linux/ar71xx/image/Makefile
将
wndr4300_mtdlayout=mtdparts=ar934x-nfc:256k(u-boot)ro,256k(u-boot-env)ro,256k(caldata),512k(pot),2048k(language),512k(config),3072k(traffic_meter),2048k(kernel),23552k(ubi),25600k@0x6c0000(firmware),256k(caldata_backup),-(reserved)
改为（将ubi和firmware增加96M，完全使用128M flash）
wndr4300_mtdlayout=mtdparts=ar934x-nfc:256k(u-boot)ro,256k(u-boot-env)ro,256k(caldata),512k(pot),2048k(language),512k(config),3072k(traffic_meter),2048k(kernel),121856k(ubi),123904k@0x6c0000(firmware),256k(caldata_backup),-(reserved)

#先编译要用到的工具和库
sudo make prereq && make tools/install && make toolchain/install
#if encounter errors: 
# Build dependency: Please install the openssl library (with development headers)
# run below first.
sudo apt-get install libssl-dev
sudo apt-get install libssl0.9.8

sudo make menuconfig

sudo make -j 5
