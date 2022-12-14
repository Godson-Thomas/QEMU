<img src="https://github.com/Godson-Thomas/Toolchain/blob/master/QEMU.PNG" width="200">  <br><br>

# [crosstool NG,uboot]


## _Installing crosstool-NG_

* Install the missing libraries during 'make' process<br><br>

```
cd ~

sudo apt-get update

git clone https://github.com/crosstool-ng/crosstool-ng.git

cd crosstool-ng

git checkout crosstool-ng-1.24.0

./bootstrap

./configure --enable-local

make

sudo make install

```

```
./ct-ng

./ct-ng distclean

```

## _Building a tool chain for QEMU_

```
./ct-ng menuconfig
```
<br>

<img src="https://github.com/Godson-Thomas/Toolchain/blob/master/menuconfig.png" width="700">  <br><br>



* Go to Paths & misc options and uncheck "Render the toolchain read-only"<br><br>

```
./ct-ng list-samples

./ct-ng arm-unknown-linux-gnueabi

./ct-ng build

```

* Error : "isl download" <br><br>
```
cd .buld/tarballs

wget https://libisl.sourceforge.io/isl-0.20.tar.gz

wget https://github.com/libexpat/libexpat/releases/download/R_2_2_6/expat-2.2.6.tar.bz2

cd ..
cd ..

```

* Build again <br><br>

```
./ct-ng build
```

* Error : "string in namespace does not name a type" <br><br>
```
cd ~/crosstool-ng/.build/arm-unknown-linux-gnueabi/src/binutils/gold

gedit errors.h
```
* Include the string header file. Build again <br><br>


```
./ct-ng build

```

```
cd ..

cd ~/x-tools/arm-unknown-linux-gnueabi/bin

ls 
```
* Here tools such as compiler,linker and debugger are located.


### _Analyzing compiler configuration_

```
cd ~/x-tools/arm-unknown-linux-gnueabi/bin

./arm-unknown-linux-gnueabi-gcc -v
```









## _Embedded Linux Boot Process_

<br>

<img src="https://github.com/Godson-Thomas/Toolchain/blob/master/elinuxboot.PNG" width="700">  <br><br>




## _Building U-boot : QEMU_


```
git clone git://git.denx.de/u-boot.git

cd u-boot

cd ~/u-boot/configs

ls 
```
* configurations for the processors
```
 
cd ~/u-boot/configs
 
 cat qemu_arm_defconfig
```
## _Cross Compilation_
```
 
 PATH=${HOME}/x-tools/arm-unknown-linux-gnueabi/bin/:$PATH
 
 export CROSS_COMPILE=arm-unknown-linux-gnueabi-
 
 export ARCH=arm
 
 make CROSS_COMPILE=arm-unknown-linux-gnueabi- \qemu_arm_defconfig
 
 make CROSS_COMPILE=arm-unknown-linux-gnueabi-

```

* u-boot.bin, u-boot.img files will be created.

## _Testing u-boot with QEMU_

* Install QEMU
```
sudo apt-get install -y qemu-kvm qemu virt-manager virt-viewer libvirt-bin
```


```
cd ~/u-boot

qemu-system-arm -M virt -nographic -no-reboot -bios u-boot.bin
```

# [Running custom Linux kernel in Qemu: x86_64 based architecture]

# _Cross compilation setup_



```
$ export ARCH=x86_64
```

```
$ make x86_64_defconfig
```

```
$ make menuconfig
```

![enabling_compiler](https://user-images.githubusercontent.com/60534517/207508941-af0ddf3b-31d3-4915-82c0-bf456e30ac25.png)


![enabling_compiler_debug2](https://user-images.githubusercontent.com/60534517/207509005-c262c5b0-6340-4046-8aa2-410cf1004826.png)


![Enabling_](https://user-images.githubusercontent.com/60534517/207509733-b3a3eeaf-7592-4080-9bb8-3b448f356f4d.png)


```
$ make
```

## _Setting up Qemu_

$ sudo apt install qemu qemu-system


$ qemu-system-x86_64 -no-kvm -kernel arch/x86/boot/bzImage -hda /dev/zero -append "root=/dev/zero console=ttyS0" -serial stdio -display none

- Error: Root file system mount

# Creating a root file system from buildroot project

```
$ git clone git://git.buildroot.net/buildroot
$ cd buildroot
```

```
$ make menuconfig

```
* Select architecture and file system type


![Arch](https://user-images.githubusercontent.com/60534517/207510538-8164efe2-6593-48d8-9b00-2bd6458ac1aa.png)<br>


![rootfs](https://user-images.githubusercontent.com/60534517/207510737-e2b92228-7456-4e04-b63d-9898d91d8209.png)

```
$ make
```
## _Run qemu with own Linux Kernel and Root file system_

```
qemu-system-x86_64 -kernel arch/x86/boot/bzImage -boot c -m 2049M -hda ../buildroot/output/images/rootfs.ext4  -append "root=/dev/sda rw console=ttyS0,115200 acpi=off nokaslr" -serial stdio -display none
```

Source Link: https://medium.com/@daeseok.youn/prepare-the-environment-for-developing-linux-kernel-with-qemu-c55e37ba8ade
