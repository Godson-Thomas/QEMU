<img src="https://github.com/Godson-Thomas/Toolchain/blob/master/QEMU.PNG" width="500">  <br><br>


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

<img src="https://github.com/Godson-Thomas/Toolchain/blob/master/menuconfig.png" width="500">  <br><br>



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

<img src="https://github.com/Godson-Thomas/Toolchain/blob/master/elinuxboot.PNG" width="500">  <br><br>




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

* Install Qemu
```
sudo apt-get install -y qemu-kvm qemu virt-manager virt-viewer libvirt-bin
```


```
cd ~/u-boot

qemu-system-arm -M virt -nographic -no-reboot -bios u-boot.bin
```