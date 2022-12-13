# ts7600Linux3-14-28
Update to the ts7600 OS to linux 3.14.28<br>
wget ftp://ftp.embeddedTS.com/ts-arm-sbc/ts-7680-linux/cross-toolchains/imx28-cross-glibc.tar.bz2<br>
tar xvf imx28-cross-glibc.tar.bz2<br>
#git clone https://github.com/embeddedTS/linux-3.14.28-imx28.git<br>
#<br>
sudo apt-get install build-essential libncurses5-dev libncursesw5-dev git u-boot-utils<br>

cd linux-3.14.28-imx28/<br>
git checkout 7600-4600<br>
sudo dpkg --add-architecture i386<br>
sudo apt-get update <br>
sudo apt-get install libc6-dev:i386 zlib1g-dev:i386 libstdc++6:i386<br>
<br>
export LOADADDR=0x40008000<br>
export ARCH=arm<br>
export AARCH=mx28<br>
export CROSS_COMPILE=$HOME/arm-fsl-linux-gnueabi/bin/arm-linux-<br>
make ts7600_defconfig<br>
make -j2 && make zImage && make modules && make uImage <br>
cat arch/arm/boot/zImage arch/arm/boot/dts/imx28-ts7600-256M.dtb > zImage<br> 
mv zImage arch/arm/boot/<br>
./build_bootstream <br>
patch -p1 < install_bootstream-newer-fdisk.patch <br>
./install_bootstream imx-bootlets-src-10.12.01/imx28_ivt_linux.sb sdb 1<br>
./install_hdr_mod sdb2<br>
