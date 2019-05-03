# linux-ck-reiser4 (current kernel base = 5.0.10)
Arch Linux custom kernel with Reiser4 filesystem patches, -ck1 patchset featuring MuQSS v.0190, Graysky's GCC patch for additional CPU optimizations and other options in the PKGBUILD.
The kernel also has options for a already optimized kernel configuration for users of the Dell Precision >=5520/Dell XPS 15 >=9560 laptops.

PLEASE take a CLOSE look at the PKGBUILD before compiling and/or running the kernel.

## Features:

* Optional Reiser4 filesystem support (Thanks to @edward6 - Edward Shishkin, enabled by default)
* Optional support for Con Kolivas -ck1 patchset (Thanks to Con Kolivas, enabled by default)
* Optional support for disabling the MuQSS scheduler provided by the -ck1 patchset (enabled by default)
* LZ4 compression (enabled by default)
* Optional Intel-only CPU support (enabled by default)
* Optional support for AMD CPUs (disabled by default)
* Optional support for NUMA (disabled by default, inspired by @sudokamikaze)
* Additional CPU optimizations (Thanks to @graysky, default=native)
* Optional support for disabling/enabling Nouveau support (disabled by default)
* Optional support for disabling/enabling watchdog timers support (disabled by default)
* Optional support for kernel configuration through 'nconfig' (disabled by default)
* Optional support for a very lightweight kernel config for Dell Precision >=5520/Dell XPS 15 >=9560 laptops (disabled by default)


# HOWTO

Inside the PKGBUILD you will find the following options:

_makenconfig=

Set this to "y" to enable custom kernel configuration (based on the provided configuration) to allow for custom user configurations.

_amdcpu=

Set this to "y" if you plan on running the kernel on a AMD CPU. The default is Intel *ONLY*.

_enable_native=y

This will detect the current CPU subarchitecture and compile the kernel with "-march=native" to allow for the best possible optimization possible. Because the use of Graysky's GCC patch, there is also alot of preconfigured subarchitectures available in "nconfig" (Intel Haswell, Broadwell, Skylake etc.).

_reiser4=y

This will enable the Reiser4 filesystem patches and set the appropriate kernel configuration to enable it. Recommended.

_disable_numa=y

This will disable the default NUMA support for x64 (not available for x86) which will lower kernel overhead as NUMA is not needed on systems that does not have multiple physical CPUs (not threads or cores).

_disable_nouveau=y

This will disable the compilation of the Nouveau open-source NVIDIA driver. Simply remove the "y" to enable it if you use it.
Many people experience problems and/or have to blacklist this module in order to run bumblebee or the proprietary NVIDIA drivers.

_enable_watchdog=

This will by default not enable any watchdog support in the kernel, thus eliminating alot of unneeded kernel modules which is often not wanted by the average user any way. Enable by setting "y" if you are a kernel debugger.

_enable_ck=y

Enable the performance patchset by Con Kolivas which includes the MuQSS scheduler (previously known as the BFS scheduler) for increased interactivity and responsiveness especially for desktop systems. RECOMMENDED.

_enable_muqss=y

To provide a little bit of flexibility, I added the option to allow for disabling the MuQSS scheduler provided by the -ck1 patchset if experiencing problems with it. Disabling this is not recommended unless you experience problems.

_dellconfig=

Enable a custom, very lightweight and already optimized kernel configuration for users of Dell Precision 5520 or Dell XPS 9560 laptops (with Intel WiFi). 

This is not recommended without configuring it manually and using it as a template. 
In either case, enabling _makenconfig=y is highly recommended to enable the drivers that have been disabled (like touchscreen support, btrfs/xfs/jfs/f2fs support, Intel only wifi through PCIe).

For most people, the default options should suffice and to build the kernel and the kernel modules:

`` git clone https://github.com/petter3k/linux-ck-reiser4.git ``

`` cd linux-ck-reiser4 ``

`` makepkg -c ``

`` sudo pacman -U linux-ck-reiser4-5.0.10-1.pkg.tar.xz linux-ck-reiser4-headers-5.0.10-1.pkg.tar.xz ``

Or to download and install from the [AUR](https://aur.archlinux.org/packages/linux-ck-reiser4):

``yaourt -S linux-ck-reiser4``

(using yaourt in this example - any AUR helper will do)
