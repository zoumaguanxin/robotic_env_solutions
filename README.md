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
