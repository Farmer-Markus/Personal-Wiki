# Android

## Build android kernel standalone

I wanted to use my Samsung A21S as usb keyboard but the required kernel drivers were not included in LineageOS. <br>
**IMPORTANT** you **will** need the kernel that supports your device and replace device specific parts of the commands! <br>
I downloaded the [kernel](https://github.com/LineageOS/android_kernel_samsung_exynos850) for my device from Github and installed following packages:
``` bash
sudo pacman -S clang llvm make android-tools 
```

Inside the kernel repo, we need get the default config:
``` bash
make ARCH=arm64 exynos850-a21snsxx_defconfig
# If you don't want to deal with hundreds of config questions also run:
make ARCH=arm64 olddefconfig
# to set all missing configs with the default conf
```

To get the kernel to build we also need to generate the vdso's (Some faster dynamic linking stuff):
``` bash
make ARCH=arm64 CC=clang LLVM=1 CROSS_COMPILE=aarch64-linux-gnu- vdso_prepare
```

Change the kernel config to your liking and may run the **olddefconfig** again.
``` bash
make ARCH=arm64 menuconfig
```

Now to build the kernel for 64 bit arm using all available cores:
``` bash
make -j$(nproc) ARCH=arm64 CC=clang LLVM=1 CROSS_COMPILE=aarch64-linux-gnu- Image.gz
```

