🔰 Yocto First Hands-On (Beginner → Confident)

0️⃣ Host Setup (One-Time)

sudo apt update

sudo apt install -y \
  gawk wget git diffstat unzip texinfo gcc \
  build-essential chrpath socat cpio python3 \
  python3-pip python3-pexpect xz-utils \
  debianutils iputils-ping libsdl1.2-dev \
  xterm zstd lz4


Verify Python:

python3 --version

1️⃣ Create Yocto Workspace
mkdir ~/yocto
cd ~/yocto

2️⃣ Clone Poky (Yocto Reference Distribution)

👉 Use LTS branch (recommended)

git clone -b kirkstone https://git.yoctoproject.org/poky.git

cd poky

Check branch:

git branch

3️⃣ Initialize Build Environment
source oe-init-build-env


📌 This creates:

build/
 ├── conf/
 │   ├── local.conf
 │   └── bblayers.conf


You are now inside:

~/yocto/poky/build

4️⃣ First Build (QEMU – safest start)
Set machine (edit config)
nano conf/local.conf


Set:

MACHINE = "qemux86-64"


Save & exit.

Start build (this takes time ☕)
bitbake core-image-minimal


✔ This builds:

Kernel

Root filesystem

Bootable image

5️⃣ Run Image in QEMU

After build completes:

runqemu qemux86-64


You should see:

Poky (Yocto Project Reference Distro)
login:


Login as:

root

6️⃣ Verify Inside Target (Important)
uname -a
cat /etc/os-release


Expected:

NAME="Poky"

7️⃣ Add Package (Example: bash, ssh)

Edit:

nano conf/local.conf


Add:

IMAGE_INSTALL:append = " bash openssh"


Rebuild:

bitbake core-image-minimal


Run again:

runqemu qemux86-64


Check:

bash --version
ssh

8️⃣ Create Your First Custom Layer (Very Important)
bitbake-layers create-layer ../meta-mycompany


Add layer:

bitbake-layers add-layer ../meta-mycompany


Verify:

bitbake-layers show-layers

9️⃣ Create Your First Recipe
mkdir -p ../meta-mycompany/recipes-example/hello
nano ../meta-mycompany/recipes-example/hello/hello.bb


Paste:

SUMMARY = "Simple Hello App"
LICENSE = "MIT"
SRC_URI = "file://hello.c"

S = "${WORKDIR}"

do_compile() {
    ${CC} hello.c -o hello
}

do_install() {
    install -d ${D}${bindir}
    install -m 0755 hello ${D}${bindir}
}


Add source:

mkdir files
nano files/hello.c

#include <stdio.h>
int main() {
    printf("Hello from Yocto!\n");
    return 0;
}

🔟 Add Your App to Image

Edit:

nano conf/local.conf


Add:

IMAGE_INSTALL:append = " hello"


Build:

bitbake core-image-minimal


Run:

runqemu qemux86-64


Inside target:

hello


✔ Output:

Hello from Yocto!

1️⃣1️⃣ Clean & Rebuild (Must Know)
bitbake -c clean hello
bitbake -c cleansstate hello

1️⃣2️⃣ Where Outputs Are Stored
tmp/deploy/images/qemux86-64/


Key files:

.wic → disk image

bzImage → kernel

.rootfs.ext4 → rootfs
