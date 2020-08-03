# AprilTag_MultiPlatform
This repository contains AprilTag source code, which can be compiled as a library used in Ubuntu, Windows, Mac etc.
## Official version  
+ Official version is [here](https://github.com/AprilRobotics/apriltag.git). The aprialtag used in this multiplatform demo is the current latest version(2020-08-03). If you want to get access to the newest version, please visit official git.  
## Some reference
+ To construct this demo, i have conculted an [older crossplatform version](https://github.com/s-trinh/AprilTag), which is constructed based an older apriltag(2016-12-01).
## Get the library by yourself
+ In different platforms, you can follow the different guidance. through all guidance use cmake, so make sure your platform has **CMAKE** firstly.  
+ **Windows/Ubuntu/Mac**
```
git clone https://github.com/tzhangZJU/AprilTag_MultiPlatform.git
cd AprilTag_MultiPlatform
mkdir build
cd build
cmake ..
cmake --build .
cmake --build . --target install
```
After that, then you can find the apriltag library and include files in install directory. Now please enjoy the libraty compiled by yourself in different platforms. 
