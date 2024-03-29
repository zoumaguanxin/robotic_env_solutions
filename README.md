# system setting

## Set the Proxy for APT on Ubuntu 16.04

```
sudo vim /etc/apt/apt.conf
```
add the following
```
Acquire::http::Proxy "http://127.0.0.1:41091";
Acquire::https::Proxy "https://127.0.0.1:41091";
```
you shoul use the port instead 41091 that your vpn use actually.

对于一些秘钥无法curl下载，可以网页下载下来后手动添加。或者为curl 和 wget等都设置代理

```
# 修改shell配置文件 ~/.bashrc ~/.zshrc等
export http_proxy=socks5://127.0.0.1:1024
export https_proxy=$http_proxy

# 设置setproxy和unsetproxy 可以快捷的开关
# 需要时先输入命令 setproxy
# 不需要时输入命令 unsetproxy
alias setproxy="export http_proxy=socks5://127.0.0.1:1024; export https_proxy=$http_proxy; echo 'HTTP Proxy on';"
alias unsetproxy="unset http_proxy; unset https_proxy; echo 'HTTP Proxy off';"
```


## 修改curl配置文件

vim ~/.curlrc
```
socks5 = "127.0.0.1:1090"
```
注意，设置了代理可能会导致某些项目使用curl访问远程服务时无法连接，此时要关闭代理

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
### Bug 3:

有关pcl的一些程序，在一些电脑上，程序并没有写错，但总是在运行结束或者函数返回的出现地方段错误。修改以下编译选项可以解决：
```
set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_FLAGS "-std=c++11")
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -Wall -g")
```

## opencv

## RTABMap
### recommened denpencies
```
sudo apt-get install ros-kinetic-rtabmap ros-kinetic-rtabmap-ros
sudo apt-get remove ros-kinetic-rtabmap ros-kinetic-rtabmap-ros
```
**g2o**
**GTSAM**
**libpointmatcher**

### Install Required Denpendies

pcl version > 1.8 is recommended strongly

## maplab


## cuda

### nvidia-driver

首先查看显卡是否挂载成功。有时候重启可能会掉，最好每次都检查
```
lspci | grep -i nvidia
```

貌似有一些驱动和ubuntu某些内核不兼容，如果一下方法始终无法成功安装时，可以考虑使用低一点版本的nvidia驱动

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
Click [here](https://www.nvidia.com/Download/index.aspx?lang=cn) to download a driver matched with `GeFore MX150`. note that you should download the version relative to your system version, eg, ubuntu 18.04 or 16.04. Dismatched system version also will cause your installation failure.

And then, there are some necessary prepration.

#### 1. add others graphic drivers to blacklists

sudo gedit /etc/modprobe.d/blacklist.conf 

Append the following:

```
blacklist nouveau
options nouveau modeset=0
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

有一些主板不支持关闭安全启动，考虑关闭安全启动内容中其他相关选项。又或者可以考虑使用如下方法：
```
sudo mokutil --disable-validation
```

if you don't disable it, there will be notation about creating a trusted key when installing nvidia driver. Then you chooese creating it, it seems to be necessary to add the key to somewhere. So That mean that you might failed to install nvidia driver.

### 3. note `-no-opengl-files` parameter

alt+ctrl+fn+F1 to `command line mode`. some computer fn is not necessary.

```

sudo service ligthdm stop //如果显示不能加载，则忽略这一步骤

sudo ./NVIDIA_DRIVER.run -no-opengl-files -no-x-check -no-nouveau-check --disable-nouveau

```

no atuomatically build kernel module.

ignore gcc version.



```
Would you like to register the kernel module souces with DKMS? This will allow DKMS to automatically build a new module, if you install a different kernel later? 
NO 大多数情况都可以选择no
YES 需要先安装好dkms, 选择yes有一个好处就是当内核升级时不需要再重新安装显卡。

```
```
Nvidia's 32-bit compatibility libraries?

No
```

```
Would you like to run the nvidia-xconfigutility to automatically update your x configuration so that the NVIDIA x driver will be used when you restart x? Any pre-existing x confile will be backed up.

YES
```


if installation is ok, you will accept a note that the installation is complete.

### 4 CHECK

```
modprobe nvidia
```

nvidia-smi (show mission).

如果出现无法交互，考虑安装dkms, 
```
sudo apt-get install dkms
ls /usr/src
sudo dkms install -m nvidia -v 418.56(对应自己安装的版本号)
```
如果以上方法都无法成功，则考虑换ubuntu版本或者nvidia驱动版本

## cuda

```
nvidia-smi
```

you will find the matched cuda version. and then goto cuda [web](https://developer.nvidia.com/cuda-toolkit-archive) to download related run file. cuda版本和nvidia驱动有一定对应关系，一般来讲，cuda版本越高，通常只兼容更高版本的nvidia驱动。cuda一般要求nvida版本驱动大于等于多少。


## Eigen

Eigen is used widely in robotic repos. Such as, ceres, sophus, pangolin, pcl, and some ros packages, and so on. When you create your projects, you must be careful that you should use the same Eigen version. That mean the main project and its all dependencies that depend on Eigen library should comiple with same Eigen version. Using different eigen version might cause losts of bug in compiling period or link period, of course, sometimes you might be fortunate. if you didn't aware this point, debuging will be a long period. 

A case:

Eigen3.3.7 are used as our main project dependencies. Before starting projects,my pangolin is installed using Eigen 3.3.2, but I didn't use it, so I ignore it. A new fuction that depends on pangolin library needed to be assembled into my project. When I commplied it, a link error happened. The solution is recomipling pangolin with Eigen 3.3.7.

Second case:

This case is considerably implicated. I make sure that I use an only one sophus library, but the complier noted me that the opreator* about SE3 can not be used correctly. In fact, because I used pcl_ros package that default include Eigen 3.3.2, at the smae time my main project includes eigen 3.3.7, so when I call SE3<T>*SE3<T>, the template parameter T of two `SE3<T>` didn't use same T. SE3 * SE3 didn't work. Of course, reducing the pcl_ros is a solution instead of making pcl_ros include Eigen 3.3.7.  



### 1 ld error: undefined reference for Eigen::MatrixBase

### solution C1:
Make sure that all depencies use a same Eigen Version. A simple solution is to retain only one high version Eigen. You can install it in 'usr/' directory to cover old version














