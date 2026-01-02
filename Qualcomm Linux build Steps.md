
============================================================
 Qualcomm Linux (Yocto) Build – Step-by-Step Commands
 Based on Provided Video Workflow
============================================================

HOST SYSTEM REQUIREMENTS
------------------------
Ubuntu 20.04 / 22.04 (Recommended)

Install required packages:
```
sudo apt update
sudo apt install -y gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping libsdl1.2-dev xterm zstd lz4 libncurses5
```
------------------------------------------------------------

WORKSPACE SETUP
---------------
Create Qualcomm working directory:
```
mkdir ~/qualcomm
cd ~/qualcomm
```
------------------------------------------------------------

REPO TOOL INSTALLATION
---------------------
Install Google repo tool:
```
mkdir -p ~/bin
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
export PATH=~/bin:$PATH
```
Verify:
```
repo --version
```
------------------------------------------------------------

QUALCOMM BSP INITIALIZATION (YOCTO)
----------------------------------
Initialize Qualcomm Yocto manifest (example):
```
repo init -u https://git.codelinaro.org/clo/qsdk/manifest.git -b kirkstone
```
Sync sources:
```
repo sync -j$(nproc)
```
NOTE:
Branch and manifest depend on Qualcomm SoC (QCS, RB, SM series)

------------------------------------------------------------

YOCTO BUILD ENVIRONMENT SETUP
----------------------------
Initialize build environment:
```
source setup-environment build-qcom
```
OR (older BSPs):
```
source oe-init-build-env build-qcom
```
------------------------------------------------------------

MACHINE SELECTION
-----------------
Edit machine configuration:
```
nano conf/local.conf
```
```
Example machines:
MACHINE = "qcom-qcs6490"
MACHINE = "qcom-sm8250"
MACHINE = "qcom-rb5"
```
------------------------------------------------------------

BUILD QUALCOMM LINUX IMAGE
--------------------------
Minimal image:
```
bitbake qcom-image-minimal
```
Full reference image:
```
bitbake qcom-image-full
```
------------------------------------------------------------

BUILD KERNEL ONLY
-----------------
```
bitbake virtual/kernel
```
Kernel output location:
```
tmp/deploy/images/<machine>/
```
------------------------------------------------------------

BUILD QUALCOMM FIRMWARE
-----------------------
```
bitbake qcom-firmware
```
OR:
```
bitbake firmware-qcom
```
------------------------------------------------------------

SDK GENERATION (APPLICATION DEVELOPMENT)
----------------------------------------
Generate SDK:
```
bitbake qcom-image-minimal -c populate_sdk
```
SDK path:
```
tmp/deploy/sdk/
```
Install SDK:
```
./poky-glibc-x86_64-qcom-image-minimal-aarch64-toolchain.sh
```
------------------------------------------------------------

FLASHING IMAGES TO TARGET
-------------------------
Fastboot flashing:
```
fastboot flash boot boot.img
fastboot flash system system.img
fastboot reboot
```
EDL mode flashing (example):
```
sudo ./qdl --storage ufs prog_firehose_ddr.elf rawprogram.xml patch.xml
```
------------------------------------------------------------

SERIAL DEBUG CONSOLE
--------------------
```
minicom -D /dev/ttyUSB0 -b 115200
```
------------------------------------------------------------

CLEAN AND REBUILD COMMANDS
--------------------------
Clean kernel:
```
bitbake virtual/kernel -c clean
```
Clean image:
```
bitbake qcom-image-minimal -c cleansstate
```
------------------------------------------------------------

OUTPUT ARTIFACTS LOCATION
-------------------------
tmp/deploy/images/<machine>/

Common files:
- boot.img
- system.img
- Image.gz
- dtb
- rootfs.ext4

------------------------------------------------------------

END OF FILE
============================================================
