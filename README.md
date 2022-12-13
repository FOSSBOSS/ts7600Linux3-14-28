# ts7600Linux3-14-28
Update to the ts7600 OS to linux 3.14.28
wget ftp://ftp.embeddedTS.com/ts-arm-sbc/ts-7680-linux/cross-toolchains/imx28-cross-glibc.tar.bz2
tar xvf imx28-cross-glibc.tar.bz2
#git clone https://github.com/embeddedTS/linux-3.14.28-imx28.git
#
sudo apt-get install build-essential libncurses5-dev libncursesw5-dev git u-boot-utils

cd linux-3.14.28-imx28/
git checkout 7600-4600
sudo dpkg --add-architecture i386
sudo apt-get update 
sudo apt-get install libc6-dev:i386 zlib1g-dev:i386 libstdc++6:i386

export LOADADDR=0x40008000
export ARCH=arm
export AARCH=mx28
export CROSS_COMPILE=$HOME/arm-fsl-linux-gnueabi/bin/arm-linux-
make ts7600_defconfig
make -j2 && make zImage && make modules
cat arch/arm/boot/zImage arch/arm/boot/dts/imx28-ts7600-256M.dtb > zImage 
mv zImage arch/arm/boot/
./build_bootstream 
patch -p1 < install_bootstream-newer-fdisk.patch 
./install_bootstream imx-bootlets-src-10.12.01/imx28_ivt_linux.sb sdb 1
./install_hdr_mod sdb2
