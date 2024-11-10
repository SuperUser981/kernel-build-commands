# kernel-build-commands

####################################################################################################################################
sudo apt update
sudo apt upgrade
sudo apt-get install git-all
sudo apt install gcc
sudo apt install aptitude
sudo apt install make
sudo apt install python-is-python3
sudo apt install pip
sudo apt install openssl
sudo apt install build-essential
sudo apt-get install flex
sudo apt install bison
sudo apt install android-sdk-libsparse-utils
sudo apt install bc
sudo apt-get install python3-dev
sudo apt-get install kernel-package
sudo apt-get install libssl-dev libncurses5-dev libz-dev libgdbm-dev libcdb-dev libdb-dev libncursesw5-dev libgdbm-dev libdb-dev libncursesw5-dev libssl-dev liblzma-dev libpixelfb-dev libglib2.0-dev libsqlite3-dev libxml2-utils libelf-dev



sudo apt-get install git ccache automake flex lzop bison gperf build-essential zip curl zlib1g-dev g++-multilib libxml2-utils bzip2 libbz2-dev libbz2-1.0 libghc-bzlib-dev squashfs-tools pngcrush schedtool dpkg-dev liblz4-tool make optipng maven libssl-dev pwgen libswitch-perl policycoreutils minicom libxml-sax-base-perl libxml-simple-perl lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev libgl1-mesa-dev xsltproc unzip 

####################################################################################################################################



make menuconfig

make mrproper



####################################################################################################################################

make -C $(pwd) O=$(pwd)/out <defconfig> -j2
make -C $(pwd) O=$(pwd)/out -j2




make ginkgo-stock_defconfig O=out ARCH=arm64 PATH="/workspace/Empty/proton-clang/bin:$PATH" 
make -j$(nproc --all) O=out ARCH=arm64 CC=clang CROSS_COMPILE=aarch64-linux-gnu- CROSS_COMPILE_ARM32=arm-linux-gnueabi-




make ginkgo-perf_defconfig -C $(pwd) O=out ARCH=arm64
PATH="/workspace/Empty/toolchains/clang-r428724/bin:/workspace/Empty/toolchains/arm-linux-androideabi-4.9/bin:/workspace/Empty/toolchains/aarch64-linux-android-4.9/bin:${PATH}" \
make -j$(nproc --all) O=out \
                      ARCH=arm64 \
                      CC=clang \
                      CLANG_TRIPLE=aarch64-linux-gnu- \
					  CROSS_COMPILE_ARM32=arm-linux-androideabi- \
                      CROSS_COMPILE=aarch64-linux-android-



####################################################################################################################################

build.sh

# ENV
CONFIG=vendor/sixteen_defconfig
KERNEL_DIR=$(pwd)
PARENT_DIR="$(dirname "$KERNEL_DIR")"
KERN_IMG="$HOME/out-new-R/out/arch/arm64/boot/Image.gz-dtb"
export KBUILD_BUILD_USER="elang"
export KBUILD_BUILD_HOST="kyvangkaelang"
export PATH="$HOME/toolchain/Sixteen_Clang/bin:$PATH"
export LD_LIBRARY_PATH="$HOME/toolchain/Sixteen_Clang/lib:$LD_LIBRARY_PATH"
export KBUILD_COMPILER_STRING="$($HOME/toolchain/Sixteen_Clang/bin/clang --version | head -n 1 | perl -pe 's/\((?:http|git).*?\)//gs' | sed -e 's/  */ /g' -e 's/[[:space:]]*$//' -e 's/^.*clang/clang/')"
export out=$HOME/out-new-R-miui

# Functions
clang_build () {
    make -j$(nproc --all) O=$out \
                          ARCH=arm64 \
                          CC="clang" \
                          AR="llvm-ar" \
                          NM="llvm-nm" \
			  LD="ld.lld" \
			  AS="llvm-as" \
			  OBJCOPY="llvm-objcopy" \
			  OBJDUMP="llvm-objdump" \
                          CLANG_TRIPLE=aarch64-linux-gnu- \
                          CROSS_COMPILE=aarch64-linux-gnu- \
                          CROSS_COMPILE_ARM32=arm-linux-gnueabi-
}


####################################################################################################################################



