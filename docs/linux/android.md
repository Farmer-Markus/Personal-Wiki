# Android

## Build android kernel standalone

I wanted to use my Samsung A21S as usb keyboard but the required kernel drivers were not included in LineageOS. <br>
**IMPORTANT** you **will** need the kernel that supports your device and replace device specific parts of the commands! <br>
I downloaded the [kernel](https://github.com/LineageOS/android_kernel_samsung_exynos850) for my device from Github and installed following packages:
``` bash
sudo pacman -S clang llvm make android-tools bc lld
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

## Swap existing android kernel with new one

# TODO

Use `adb root` to get root access to your phone. You might need to boot into recovery and enable adb in the advanced section <br>
Extract your boot partition

    dd if=/dev/block/by-name/boot of=/tmp/boot.img
    # Then exit from adb shell and on your pc run
    adb pull /tmp/boot.img # to pull the image to your computer

Unpack the boot image:

    unpack_bootimg --boot_img boot.img # Or path to your boot file

The 'out' dir will now contain your *kernel*, *dtb* and *ramdisk* <br>
Copy the *non*compressed kernel image to your working directory and use it in your next command to repack the boot image

    # I need to use header version 2, do some research when this is really needed
    mkbootimg --kernel new_kernel --ramdisk out/ramdisk --dtb out/dtb --header_version 2 -o boot_new.img

Now put the new ramdisk back on your phone

    adb push boot_new.img /tmp/boot_new.img
    # Login back into the root shell
    adb shell
    # And flash back to your boot partition. IMPORTANT: There is NO going back, your device might fail to boot after this but you can still go into recovery, enable adb and flash your original boot.img again using the same command!
    dd if=/tmp/boot_new.img of=/dev/block/by-name/boot
    reboot # Reboot and watch it working ... or break

