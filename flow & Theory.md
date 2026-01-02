## What is Yocto?

Yocto Project is an open-source build system used to create custom embedded Linux distributions for specific hardware (SoCs/boards).

**Why Yocto is required**

| Problem         | Without Yocto | With Yocto   |
| --------------- | ------------- | ------------ |
| OS size         | Huge          | Minimal      |
| Boot time       | Uncontrolled  | Optimized    |
| BSP integration | Manual        | Layer-based  |
| Rebuilds        | Painful       | Reproducible |
| Product scaling | Hard          | Easy         |


## What is Poky?
- Poky is the reference distribution of the Yocto Project.
- Poky as a starter kit that already includes everything you need to build an embedded Linux system using Yocto.

**Why Poky exists**

- Yocto defines standards (BitBake, layers, recipes).
- Poky provides a ready-made, working implementation of those standards.

**What Poky contains**

👉 Poky = BitBake + OE-Core + meta-yocto

Poky bundles four core components:

**1️⃣ BitBake (build engine)**

**2️⃣ OpenEmbedded-Core (meta)**

**3️⃣ Metadata & Configuration**

**4️⃣ Reference Tools & Scripts**

## **1️⃣ BitBake (build engine)**

- BitBake is the tool that actually builds everything in Yocto.
- BitBake is a task executor and build engine used by Yocto to read recipes and build embedded Linux systems automatically.

**What does BitBake do?**

BitBake:
- Reads Yocto recipes (.bb files)
- Downloads source code
- Compiles software
- Installs files into root filesystem
- Creates:
  - Linux kernel
  - Root filesystem
  - Bootloader
  - Images (.wic, .sdimg, .ext4, etc.)

## **2️⃣ OpenEmbedded-Core (meta)**

**What is OpenEmbedded Core?**

- OpenEmbedded Core (OE-Core) is the base set of recipes, classes, and configuration files used to build embedded Linux systems.

**What does OE-Core contain?**

OE-Core provides:
- Essential recipes (.bb) for:
  - GCC, glibc, busybox
  - core utilities (bash, coreutils)

- Common classes (.bbclass)
(how to compile, install, package)
- Default configuration
- Basic Linux userspace

## **3️⃣ Metadata & Configuration**

**1️⃣ What is Metadata?**

Metadata and configuration in Yocto define what to build, how to build it, and for which target using recipes, classes, and config files.

Metadata = build instructions + settings

It includes:
- Recipes (.bb)
- Classes (.bbclass)
- Configuration files (.conf)
- Layer info

All of this together guides BitBake.

**2️⃣ Important Metadata files (with simple meaning)**

📄 Recipe (.bb)

  - What to build
Contains:

- source URL
- dependencies
- compile & install steps

📄 Class (.bbclass)

  - How to build
  - Common logic reused by recipes.

📄 Configuration (.conf)

  - Build settings

Controls:
- target machine
- distro features
- compiler flags
- image type

**3️⃣ Key Configuration Files (VERY IMPORTANT)**

🔹 local.conf

- User-specific settings

Used to:
- choose target machine
- enable features
- speed up builds

🔹 bblayers.conf

- Which layers are enabled

🔹 layer.conf

  - Layer-level metadata

Defines:
- layer priority
- where recipes are located
- compatibility

## **4️⃣ Reference Tools & Scripts**

These are helper tools and scripts provided by Yocto to make development, debugging, and maintenance easier.

👉 They don’t replace BitBake
👉 They sit around BitBake and help you work faster and cleaner

- BitBake = build engine
- devtool = fast development
- runqemu = test without hardware
- toaster = web UI
- SDK tools = app development

## Layers
**What is a Layer?**

- A layer is a collection of recipes + configs.

**Why layers?**

- Modular
- Easy customization
- Clean separation

**Common layers**

| Layer             | Purpose          |
| ----------------- | ---------------- |
| meta              | Core recipes     |
| meta-poky         | Reference distro |
| meta-bsp          | Board support    |
| meta-openembedded | Extra packages   |
| meta-custom       | Your own code    |

## Parsing Metadata

**What happens**

BitBake reads:

- .bb (recipes)
- .bbappend
- .conf
- .inc

**Result**
- Dependency graph
- Task graph
- If syntax errors → build stops here.

## Fetching Source Code (do_fetch)
**What happens**

Downloads source from:
- Git
- Tarballs
- Local files

**Example:**
```
SRC_URI = "git://github.com/u-boot/u-boot.git;branch=master"
```
## Unpack (do_unpack)
**What happens**
- Extracts source code
- Creates working directory

## Patch (do_patch)
**What happens**

- Applies Yocto patches
- Uses quilt internally

Why important:
- Board fixes
- Feature enable/disable
- Security fixes

## Configure (do_configure)
**What happens**

- Runs autotools / cmake / meson
- Generates Makefiles

## Compile (do_compile)

**What happens**

- Cross-compiles code
- Uses cross-toolchain

## Install (do_install)

**What happens**

- Installs files into staging directory
- NOT into target rootfs yet

## Package (do_package)

## Root Filesystem (do_rootfs)

**What happens**

- Combines packages
- Resolves dependencies
- Creates root filesystem


# 🔑 IMPORTANT YOCTO KEYWORDS (WITH SIMPLE EXPLANATION)

| Keyword         | Meaning                |
| --------------- | ---------------------- |
| BitBake         | Build engine           |
| Recipe (.bb)    | How to build a package |
| Layer           | Collection of recipes  |
| Metadata        | Build instructions     |
| Task            | Individual build step  |
| MACHINE         | Target hardware        |
| DISTRO          | Policy & defaults      |
| BSP             | Board support package  |
| Sysroot         | Staging rootfs         |
| Cross Toolchain | Compiler for target    |
| do_fetch        | Download source        |
| do_compile      | Build source           |
| do_install      | Stage files            |
| do_rootfs       | Build root filesystem  |
| Image           | Final output           |
