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

1. Disable security boot

When your computer is powering on, press F2 to BIOS center. Find security boot. Disable it. 
if you didn't do this, the nvidia-smi will not work

2. install

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

if you don't disable it, there will be notation about creating a trusted key when installing nvidia driver. Then you chooese creating it, it seems to be necessary to add the key to somewhere. So That mean that you might failed to install nvidia driver.

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

you will find the matched cuda version. and then goto cuda [web](https://developer.nvidia.com/cuda-toolkit-archive) to download related run file.


## Common Bug

Eigen is used widely in robotic repos. Such as, ceres, sophus, pangolin, pcl, and some ros packages, and so on. When you create your projects, you must be careful that you should use the same Eigen version. That mean the main project and its all dependencies that depend on Eigen library should comiple with same Eigen version. Using different eigen version might cause losts of bug in compiling period or link period, of course, sometimes you might be fortunate. if you didn't aware this point, debuging will be a long period. 

A case:

Eigen3.3.7 are used as our main project dependencies. Before starting projects,my pangolin is installed using Eigen 3.3.2, but I didn't use it, so I ignore it. A new fuction that depends on pangolin library needed to be assembled into my project. When I commplied it, a link error happened. The solution is recomipling pangolin with Eigen 3.3.7.

Second case:

This case is considerably implicated. I make sure that I use an only one sophus library, but the complier noted me that the opreator* about SE3 can not be used correctly. In fact, because I used pcl_ros package that default include Eigen 3.3.2, at the smae time my main project includes eigen 3.3.7, so when I call SE3<T>*SE3<T>, the template parameter T of two `SE3<T>` didn't use same T. SE3 * SE3 didn't work. Of course, reducing the pcl_ros is a solution instead of making pcl_ros include Eigen 3.3.7.  

### 1 ld error: undefined reference for Eigen::MatrixBase

### solution C1:
Make sure that all depencies use a same Eigen Version














