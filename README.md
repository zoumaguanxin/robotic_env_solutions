# robotic_repos_compilation_solutions


## pcl

### Install Required Dependencies

```
sudo apt-get install libflann1.8 libflann-dev
sudo apt-get install libeigen3-dev
sudo apt-get install libboost-all-dev
if qt4
sudo apt-get install libvtk5.10-qt4 libvtk5.10 libvtk5-dev
if qt5
sudo apt-get install libvtk6-dev libvtk6-qt-dev  
```

### bug 1:

```
undefined reference to `pcl::search::Search<pcl::PointXYZRGBNormal>::getName[abi:cxx11]() const
```

### solution:

add the following code or install pcl version > 1.8.0
```
#include <pcl/search/impl/search.hpp>
```

### bug 2:
```
can not find QVTK_LIBRARY
```
### solution:
```
sudo apt-get install libvtk6-dev libvtk6-qt-dev  
```

## opencv

## RTABMap

### Install Required Denpendies

pcl version > 1.8 is recommended strongly

## maplab


## cuda

### nvidia-driver

**desktop (recommended)**

```

sudo add-apt-repository ppa:graphics-drivers/ppa

sudo apt update

ubuntu-drivers devices 

sudo ubuntu-drivers autoinstall

or

sudo apt-get install nvidia-xxx

```

or by software & update

`Additional Drivers`

**notebook(recommended method)**

lspci | grep NVIDIA

eg.

```
NVIDIA Corporation GP108M [GeForce MX150]
```
Click [here](https://www.nvidia.com/Download/index.aspx?lang=cn) to download a driver matched with `GeFore MX150`.

And then, there are some necessary prepration.

#### 1. add others graphic drivers to blacklists

sudo gedit /etc/modprobe.d/blacklist.conf 

Append the following:

```
blacklist vga16fb
blacklist nouveau
blacklist rivafb
blacklist rivatv
blacklist nvidiafb
```
```
sudo update-initramfs -u
sudo reboot
```

```
lsmod | grep nouveau
```

### 2. disable security boot

When your computer is powering on, press F2 to BIOS center. Find security boot. Disable it.

if you don't disable it, there will be notation about creating a trusted key when installing it. After creating it, it seems to be necessary to add the key to somewhere.

### 3. note `-no-opengl-files` parameter

alt+ctrl+fn+F1 to `command line mode`. some computer fn is not necessary.

```

sudo service ligthdm stop

sudo ./NVIDIA_DRIVER.run -no-opengl-files

```

no atuomatically build kernel module.

ignore gcc version.

no 32 compatiable.

install without register kernel module.

if installation is ok, you will accept a note that the installation is complete.

### 4 CHECK

nvidia-smi (show mission).


## cuda

```
nvidia-smi
```

you will find the matched cuda version. and then goto cuda web to download related run file.


## Common Bug

### 1 ld error: undefined reference for Eigen::MatrixBase

### solution C1:
Make sure that all depencies use a same Eigen Version

















