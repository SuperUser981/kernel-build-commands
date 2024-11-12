# open kernel-build-commands.txt

```
Building a Kernel for Beginners 

Summary 
1. Download source code 
2. Expand to folder 
3. Push to github.com 
4. Modify (add changes and cherry-picks)
5. Push changes to github.com 
6. Compile kernel 
7. Add Image file to boot.img or add to zip installer





Requirements
---------------------
You need a PC with Linux
A compatible kernel (4.4 or 4.9 LTS work best)
arm64 or x86_64
Patience

0. Setup Linux
---------------------

Open terminal 

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
sudo apt-get install bc
sudo apt-get install python3-dev
sudo apt-get install kernel-package
sudo apt-get install libssl-dev libncurses5-dev libz-dev libgdbm-dev libcdb-dev libdb-dev libncursesw5-dev libgdbm-dev libdb-dev libncursesw5-dev libssl-dev liblzma-dev libpixelfb-dev libglib2.0-dev libsqlite3-dev libxml2-utils libelf-dev
sudo apt-get install gcc-aarch64-linux-gnu
sudo apt-get install g++-aarch64-linux-gnu
sudo apt-get install cpio

sudo add-apt-repository ppa:linaro-maintainers/toolchain
sudo apt-get update
sudo apt-get install gcc-**version**-aarch64-linux-gnu

sudo apt-get install git ccache automake flex lzop bison gperf build-essential zip curl zlib1g-dev g++-multilib libxml2-utils bzip2 libbz2-dev libbz2-1.0 libghc-bzlib-dev squashfs-tools pngcrush schedtool dpkg-dev liblz4-tool make optipng maven libssl-dev pwgen libswitch-perl policycoreutils minicom libxml-sax-base-perl libxml-simple-perl lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev libgl1-mesa-dev xsltproc unzip 

####################################################################################################################################


# Increase linux swapfile size (works best on SSD)

sudo swapoff -a

sudo dd if=/dev/zero of=/swapfile bs=1G count=8
sudo mkswap /swapfile
sudo swapon /swapfile

free -m

# install python 2
pip install virtualenv

export PATH=$PATH:/home/<your@name>/.local/bin

apt install python3-virtualenv

sudo apt install python2.7
sudo apt install openssl
sudo aptitude install libssl-dev

sudo apt-get install libtinfo5

# install repo

mkdir -p ~/.bin
PATH="${HOME}/.bin:${PATH}"
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/.bin/repo
chmod a+rx ~/.bin/repo

1. Download the Source Code
-------------------------------------------- (https://opensource.samsung.com/main)
Search for your device

Extract the kernel.tar.7z
extract into "Kernel" folder

2. Setup toolchains
------------------------------
Follow the instructions in README_kernel.txt
Also look at build_kernel.sh and makefile for toolchain information 
(Search for CROSS_COMPILE for gcc and CC for clang)

Search github.com and download the correct toolchains if they're not already in the toolchain folder 


Here are a few examples

https://github.com/physwizz/toolchain_cross-compile
Or
https://github.com/physwizz/compiler
Or
https://github.com/physwizz/A217m-S-SB/tree/main/toolchain

You might need to put gcc toolchain outside the kernel folder
Eg
/home/<user>/toolchains


3. Set up GitHub
------------------------
See this guide
https://t.me/physwizz2/10

Create new repo on GitHub.com
Like this one
https://github.com/physwizz/New
Copy the text section "or create a new repository on the command line"
Something like this 

echo "# New" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:physwizz/New.git
git push -u origin main
Open Terminal In The Kernel Folder

Paste the section from github
Type
git add --all
git commit -a
Type "initial"
Ctrl o
Ctrl x 
git push origin main -f







4. compile the kernel
---------------------------

1. open the defconfig file with text editor located at
/home/<user>/Kernel/arch/arm64/configs/<your_device>_defconfig

You can make a copy of this file and call it original_defconfig

Make these changes

CONFIG_LOCALVERSION="-<user>"

# CONFIG_LOCALVERSION_AUTO is not set
# CONFIG_CC_STACKPROTECTOR_STRONG is not set

# CONFIG_SECURITY_DEFEX is not set

# CONFIG_DM_VERITY is not set

# CONFIG_INTEGRITY is not set
# CONFIG_INTEGRITY_SIGNATURE is not set
# CONFIG_INTEGRITY_ASYMMETRIC_KEYS is not set
# CONFIG_INTEGRITY_AUDIT is not set

CONFIG_KERNEL_GZIP=y
Configures Output of Image.gz

$ make menuconfig




For further changes see my GitHub
https://github.com/physwizz

cd Kernel
###############################################################################
./build_kernel.sh

If you get python errors you might need to use python 2

$ make mrproper

sudo -s
virtualenv -p /usr/bin/python2.7 Vpy27
source Vpy27/bin/activate


####################################################################################################################################

4. How to compile the kernel with Clang (standalone)
NOTE: I am not going to write this for beginnings. I assume if you are smart enough to pick some commits, you are smart enough to know how to run git clone and know the paths of your system.

Add the Clang commits to your kernel source
https://gist.github.com/P1N2O/b9b2604c58aa4d7486e2fc0d327d23dc#getting-the-clang-patchset

Download/build a compatible Clang toolchain
https://gist.github.com/P1N2O/b9b2604c58aa4d7486e2fc0d327d23dc#how-to-get-a-clang-toolchain

Download/build a copy of binutils
https://gist.github.com/P1N2O/b9b2604c58aa4d7486e2fc0d327d23dc#how-to-get-binutils



Compile the kernel (for arm64, x86_64 is similar - example using AOSP's toolchains):

###############################################################################
make -C $(pwd) O=$(pwd)/out <defconfig> -j2
make -C $(pwd) O=$(pwd)/out -j2
###############################################################################

make ginkgo-stock_defconfig O=out ARCH=arm64 PATH="/workspace/Empty/proton-clang/bin:$PATH" 
make -j$(nproc --all) O=out ARCH=arm64 CC=clang CROSS_COMPILE=aarch64-linux-gnu- CROSS_COMPILE_ARM32=arm-linux-gnueabi-
###############################################################################

make ginkgo-perf_defconfig -C $(pwd) O=out ARCH=arm64
PATH="/workspace/Empty/toolchains/clang-r428724/bin:/workspace/Empty/toolchains/arm-linux-androideabi-4.9/bin:/workspace/Empty/toolchains/aarch64-linux-android-4.9/bin:${PATH}" \
make -j$(nproc --all) O=out \
                      ARCH=arm64 \
                      CC=clang \
                      CLANG_TRIPLE=aarch64-linux-gnu- \
					  CROSS_COMPILE_ARM32=arm-linux-androideabi- \
                      CROSS_COMPILE=aarch64-linux-android-

###############################################################################
make ginkgo-perf_defconfig O=out ARCH=arm64 PATH="/workspace/Empty/Sixteen_Clang/bin:$PATH"
make -j$(nproc --all) O=out ARCH=arm64 CC=clang CROSS_COMPILE=aarch64-linux-gnu- CROSS_COMPILE_ARM32=arm-linux-gnueabi- CLANG_TRIPLE=aarch64-linux-gnu

####################################################################################################################################


# kernel-build-commands

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





3. Output is found in /home/<user>/Kernel/arch/arm64/boot

4. Copy Image from boot folder and Insert it into the MyKernel.zip

5. Extract Version and AnyKernel.sh and edit it then reinsert.

Use folder name found in 
/dev/block/platform 

You could also use AIK to insert the kernel into boot.img

Note: if it doesn't build and you can't see the error message, delete the -j** from the build script


========================
Physwizz group: https://t.me/physwizz3
Physwizz Channel:  https://t.me/physwizz2
https://gist.github.com/P1N2O/b9b2604c58aa4d7486e2fc0d327d23dc#how-to-get-binutils




```






