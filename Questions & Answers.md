# Yocto Project – Top-Level Interview Questions with Answers

---

## 1. What is the Yocto Project and why is it used?

The Yocto Project is an embedded Linux build framework used to create custom Linux distributions for specific hardware. It is used to gain full control over the kernel, root filesystem, packages, toolchain, and build reproducibility.

## 2. Why is Yocto not a Linux distribution?

Yocto does not provide a ready-to-run OS. Instead, it provides tools and metadata to build a Linux distribution tailored to a specific device.

## 3. What problems does Yocto solve compared to Ubuntu/Debian?

Yocto enables minimal images, cross-compilation, deterministic builds, long-term maintenance, and hardware-specific customization, which desktop distros cannot provide.

## 4. What is Poky?

Poky is the reference build system of Yocto. It contains BitBake, OpenEmbedded-Core, and sample configurations.

## 5. Relationship between Yocto, OpenEmbedded, and BitBake?

Yocto is the project, OpenEmbedded is the metadata framework, and BitBake is the build engine that executes tasks.

## 6. Where is Yocto commonly used?

Automotive, networking equipment, industrial controllers, medical devices, IoT gateways, and consumer electronics.

---

## 7. What happens when you run bitbake core-image-minimal?

BitBake parses metadata, resolves dependencies, builds packages, assembles the root filesystem, and generates a bootable image.

## 8. Explain the Yocto build flow.

Source fetch → configure → compile → install → package → rootfs creation → image generation.

## 9. What are BitBake tasks?

Tasks are build steps like do_fetch, do_compile, do_install, do_package.

## 10. How does BitBake resolve dependencies?

Through DEPENDS (build-time) and RDEPENDS (runtime) declared in recipes.

## 11. What is the work directory?

It stores per-recipe build artifacts during compilation.

## 12. Recipe vs image build?

A recipe builds one package; an image builds a complete root filesystem.

---

## 13. What is a recipe?

A .bb file that defines how to fetch, build, and package software.

## 14. What is metadata?

Instructions that describe how software is built in Yocto.

## 15. .bb vs .bbappend?

.bb defines a recipe; .bbappend modifies an existing recipe.

## 16. Why not modify upstream recipes?

Upgrades overwrite changes and break maintainability.

## 17. What is a .bbclass?

Reusable build logic shared across recipes.

## 18. What are .inc files?

Shared variables or functions included by recipes.

## 19. What is SRC_URI?

Defines where source code or patches are fetched from.

## 20. Where are sources stored?

In build/downloads/.

---

## 21. What is a layer?

A collection of recipes and configurations.

## 22. Why layers are mandatory?

They provide modularity and isolation.

## 23. Purpose of a custom layer?

To add project-specific changes without touching upstream layers.

## 24. What if upstream layers are modified?

Upgrades break and debugging becomes difficult.

## 25. Role of layer.conf?

Registers the layer and sets priority.

## 26. How layer priority works?

Higher priority layers override lower ones.

---

## 27. local.conf vs bblayers.conf?

bblayers.conf selects layers; local.conf customizes the build.

## 28. What is defined in local.conf?

Machine, package format, parallelism, image contents.

## 29. What is MACHINE?

Defines target hardware.

## 30. What if MACHINE is wrong?

Kernel, bootloader, and rootfs may not boot.

## 31. What is DISTRO?

Defines distribution-wide policies.

## 32. MACHINE vs DISTRO?

MACHINE is hardware-specific; DISTRO is policy-specific.

---

## 33. What is BSP?

Board Support Package providing hardware support.

## 34. What does machine config control?

CPU, kernel, bootloader, device tree, tuning.

## 35. How is device tree selected?

Through KERNEL_DEVICETREE in machine config.

## 36. How is kernel chosen?

Using PREFERRED_PROVIDER_virtual/kernel.

## 37. How to change kernel version?

Modify kernel recipe or use bbappend.

## 38. How to add custom device tree?

Add .dts via bbappend and update machine config.

---

## 39. What is an image recipe?

Defines which packages go into the root filesystem.

## 40. core-image-minimal vs full?

Minimal has fewer packages; full has utilities and tools.

## 41. How to add packages to image?

Using IMAGE_INSTALL.

## 42. What is IMAGE_INSTALL?

Specifies packages included in the image.

## 43. += vs :append?

:append is override-safe.

## 44. Supported filesystem formats?

ext4, wic, squashfs, tar.gz.

---

## 45. Package vs image?

Package is individual software; image is complete OS.

## 46. ipk, deb, rpm?

Different package formats supported by Yocto.

## 47. Where are package feeds?

In tmp/deploy/ipk|deb|rpm.

## 48. Package splitting?

Runtime, -dev, -dbg packages.

## 49. How to include only runtime packages?

Use default package selection.

---

## 50. What is Yocto SDK?

A standalone cross-development toolchain.

## 51. How to generate SDK?

bitbake <image> -c populate_sdk.

## 52. SDK contains?

Compiler, sysroot, headers, libraries.

## 53. Native vs cross vs SDK?

Native runs on host; cross targets device; SDK is portable.

## 54. Why SDK important?

Allows app development without full Yocto rebuild.

---

## 55. How to debug build failures?

Check logs in tmp/work and task logs.

## 56. bitbake -e?

Shows environment variables.

## 57. clean vs cleanall?

clean removes workdir; cleanall removes downloads too.

## 58. Force rebuild?

bitbake -c clean <recipe>.

## 59. Where are logs?

In tmp/work/*/temp/.

## 60. Purpose of tmp/work?

Stores per-recipe build data.

---

## 61. How to reduce image size?

Remove packages, disable features, use minimal images.

## 62. Reduce build time?

Use parallelism and sstate cache.

## 63. Reduce boot time?

Disable services, reduce kernel features.

## 64. DISTRO_FEATURES impact?

Controls system-wide features.

## 65. rm_work?

Deletes work directories to save disk space.

---

## 66. Yocto vs Buildroot?

Yocto is scalable and complex; Buildroot is simpler.

## 67. When choose Yocto?

For large, long-term projects.

## 68. Why automotive prefers Yocto?

Long-term support and compliance.

## 69. OTA support?

Yes, via swupdate, RAUC, Mender.

## 70. Real-time Linux support?

Yes, using PREEMPT-RT kernels.
