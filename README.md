## Description

Custom U-Boot source code for my OrangePi Zero, based on 2017.11 version. This configuration has bootmenu enabled by default, and autoboot set for booting a kernel from sd card. 

## Installation Instructions

The boot-loader has been tailored to be flashed and booted from SPI NOR Flash with size at least 1MB. 

#### Compile

In order to compile the source code, copy and run the commands below:

```
git clone https://github.com/WarOfDevil/u-boot_opi_zero.git
cd u-boot_opi_zero
export ARCH=arm
export CROSS_COMPILE=arm-none-eabi-
make ds_opi_zero_defconfig
make
```

#### Flash

In order to flash the boot-loader on the SPI flash we need to enter into FEL mode with the opi. We will first download and compile the sunxi utilities. With this we will create an sdcard containing a special image for the opi that can enter into FEL mode. And last flash our U-Boot image.

```
git clone https://github.com/linux-sunxi/sunxi-tools.git
cd sunxi-tools
make
```

Then prepare an empty sdcard and type:

```
dd if=sunxi-tools/bin/fel-sdboot.sunxi of=/dev/sdX bs=1024 seek=8
```

After that insert the sdcard into the opi and power it up using the usb port. Flash the U-Boot image with the following command:

```
./sunxi-fel -v -p spiflash-write 0 u-boot-sunxi-with-spl.bin
```
