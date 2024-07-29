# Installation-of-VxWorks-with-ROS2-on-Raspberry-pi-
Installation procedure of VxWorks with ROS2 on Raspberry pi

**VxWorks 7 SDK for Raspberry Pi 4**
Introduction
The VxWorks 7 SDK is a development environment dedicated to VxWorks application developers which includes the following features:

standard cross-compilation tools based on clang/LLVM which can be used to build both downloadable kernel modules (DKM) and RTP (Real Time Process) applications
simplified build management: makefile, cmake, roll-your own
target / architecture specific: includes a generic VxWorks kernel which is bootable on the target platform
header files and libraries for application development
Wind River Debugger (wrdbg)
documentation
This guide helps you get up and running with developing applications for platforms running VxWorks. You can use it for creating new applications, or just exploration of VxWorks capabilities.

Setting up the development environment
You should start by downloading a VxWorks SDK for your platform of choice from https://labs.windriver.com and unpacking it. Refer to the documentation in docs in the unpacked SDK for additional information on creating and debugging applications.

OS requirements
The SDKs are meant to run on Linux hosts. Some of the examples in this document are specific to Debian derivatives.

Prerequisite(s)
Host dependencies
On Debian derivatives, the following packages need to be installed:
````
$ sudo apt install build-essential libc6:i386
````

Having an FTP server installed on your development host will make application deployment easier and allow you to access the host file system from a VxWorks target.

To accommodate for the varying runtime configurations of the VxWorks kernel images included in the SDKs, you may be interested in using an FTP server option based on pyftpdlib.

Install pyftpdlib:
````
$ sudo apt install python-pip
$ sudo pip install pyftpdlib
````

**Booting VxWorks on Raspberry Pi 4**

To create the SD card, download the firmware from:
````
https://github.com/raspberrypi/firmware/archive/1.20200212.tar.gz
````
E.g. wget https://github.com/raspberrypi/firmware/archive/1.20200212.tar.gz

Format an SD card as a FAT32 file system and copy the contents of the "boot" directory from the downloaded firmware to the SD card.

Compile a u-boot binary for Raspberry Pi 4 and copy it to the SD card

E.g. on a Ubuntu/Debian host

````
$ sudo apt install gcc-aarch64-linux-gnu $ git clone https://gitlab.denx.de/u-boot/u-boot.git
$ cd u-boot
$ CROSS_COMPILE=aarch64-linux-gnu- make rpi_4_defconfig
$ CROSS_COMPILE=aarch64-linux-gnu- make
````

Copy u-boot.bin to the SD card as u-boot-64.bin

Copy the files from /vxsdk/sdcard/ to the SD card.

Copy the VxWorks kernel image /vxsdk/bsps/rpi_4_0_1_3_0/uVxWorks to the SD card.

Connect a USB to TTL serial cable to the Raspberry Pi

On the Raspberry Pi 4 Model, GPIOs 14 and 15 are used as UART transmit and receive pins (Mini UART).

Connect the USB to serial cable adapter between the Raspberry Pi and your PC. Then start a serial communication program (e.g. minicom) and configure the serial connection parameters as follows:
````
Bps/Par/Bits       : 115200 8N1
Hardware Flow Control : No
Software Flow Control : No
After plugging in the SD card in your Raspberry Pi and powering it up, the VxWorks kernel previously copied onto the SD card will boot automatically.

U-Boot 2020.07-rc2-00133-ged9a3aa645 (May 19 2020 - 12:49:43 +0300)

DRAM:  3.9 GiB
RPI 4 Model B (0xc03112)
MMC:   emmc2@7e340000: 0, mmcnr@7e300000: 1
Loading Environment from FAT... OK
In:    serial
Out:   serial
Err:   serial
Net:   
Warning: genet@7d580000 MAC addresses don't match:
Address in DT is                dc:a6:32:86:d8:ef
Address in environment is       dc:a6:32:07:b3:a4
eth0: genet@7d580000
Hit any key to stop autoboot:  0 
8944948 bytes read in 773 ms (11 MiB/s)
## Booting kernel from Legacy Image at 00100000 ...
   Image Name:   vxworks
   Image Type:   AArch64 VxWorks Kernel Image (uncompressed)
   Data Size:    8944884 Bytes = 8.5 MiB
   Load Address: 00100000
   Entry Point:  00100000
   Verifying Checksum ... OK
   Loading Kernel Image
   !!! WARNING !!! Using legacy DTB
## Starting vxWorks at 0x00100000, device tree at 0x00000000 ...
Target Name: vxTarget 
Instantiating /tmp as rawFs,  device = 0x1
 
 _________            _________
 \77777777\          /77777777/
  \77777777\        /77777777/
   \77777777\      /77777777/
    \77777777\    /77777777/
     \77777777\   \7777777/
      \77777777\   \77777/              VxWorks 7 SMP 64-bit
       \77777777\   \777/
        \77777777\   \7/     Release version: SR0660
         \77777777\   -      Build date: Feb  3 2021 14:26:49
          \77777777\
           \7777777/         Copyright Wind River Systems, Inc.
            \77777/   -                 1984-2021
             \777/   /7\
              \7/   /777\
               -   -------

                   Board: Raspberry Pi 4 Model B - ARMv8
               CPU Count: 4
          OS Memory Size: ~3891MB
        ED&R Policy Mode: Deployed
     Debug Agent: Started (always)
         Stop Mode Agent: Not started
       BSP Status: *** UNSUPPORTED ***

 Adding 13658 symbols for standalone.
->
````

Supported features:
SD card
USB mass storage
network, defaulting to DHCP

**Build ROS 2 and its dependencies**

Clone this repository using the master branch
````
git clone https://github.com/Wind-River/vxworks7-ros2-build.git
cd vxworks7-ros2-build
````

**Build Docker image**

A Docker (Ubuntu 22.04) based build is recommended to avoid the necessity of installing build dependencies.
````
docker build --no-cache -t vxbuild:22.04 Docker/22.04/vxbuild/.
docker build --no-cache -t vxros2build:humble Docker/22.04/vxros2build/.

docker build --no-cache --build-arg ROS_DISTRO=iron -t vxros2build:iron Docker/22.04/vxros2build/.
docker build --no-cache --build-arg ROS_DISTRO=rolling -t vxros2build:rolling Docker/22.04/vxros2build/.
````

**Download and extract the VxWorks SDK**

The 24.03 SDK for Raspberry Pi 4 shall be used from https://forums.windriver.com/t/vxworks-software-development-kit-sdk/43
````
cd ~/Downloads 
wget https://d13321s3lxgewa.cloudfront.net/wrsdk-vxworks7-raspberrypi4b-1.6.tar.bz2
mkdir ~/Downloads/wrsdk && cd ~/Downloads/wrsdk
tar -jxvf ~/Downloads//wrsdk-vxworks7-raspberrypi4b-1.6.tar.bz2 --strip 1
````

**Run Docker image**
````
cd vxworks7-ros2-build
docker run -ti -h vxros2 -v ~/Downloads/wrsdk:/wrsdk -v $PWD:/work vxros2build:humble
````

By default it runs as a user wruser with uid=1000(wruser) gid=1000(wruser), if you have different ids, run it as

````
$ docker run -ti -h vxros2 -e UID=`id -u` -e GID=`id -g` -v ~/Downloads/wrsdk:/wrsdk -v $PWD:/work vxros2build:humble
````

See Dockerfile for the complete list of environment variables

Start build
Inside the Docker container: check the ROS_DISTRO version, source the development environment, and start build
````
wruser@vxros2:/work source /wrsdk/sdkenv.sh

# check environment
wruser@vxros2:/work$ make info
DEFAULT_BUILD:      sdk unixextra asio tinyxml2 eigen libxml2 libxslt ros2 pyyaml netifaces lxml
WIND RELEASE:       24.03
ROS DISTRO:         humble
TARGET BSP:         rpi_4
TARGET ARCH:        aarch64
TARGET PYTHON:      Python3.9
HOST PYTHON:        /wrsdk/vxsdk/host/x86_64-linux/bin/python3
CMAKE:              /wrsdk/vxsdk/host/x86_64-linux/bin/cmake
CURDIR:             /work
DOWNLOADS_DIR:      /work/output/downloads
PACKAGE_DIR:        /work/pkg
BUILD_DIR:          /work/output/build
EXPORT_DIR:         /work/output/export
ROOT_DIR:           /work/output/export/root
DEPLOY_DIR:         /work/output/export/deploy
WIND_CC_SYSROOT:    /wrsdk/vxsdk/sysroot
WIND_SDK_HOST_TOOLS:/wrsdk/vxsdk/host
3PP_DEPLOY_DIR:     /wrsdk/vxsdk/sysroot/usr/3pp/deploy
3PP_DEVELOP_DIR:    /wrsdk/vxsdk/sysroot/usr/3pp/develop
````
````
wruser@vxros2:/work make
wruser@vxros2:/work exit
````

Build artifacts are in the export directory
````

wruser@vxros2:/work ls output/export/deploy/
bin  lib  share  vxscript
````

Rebuild from scratch
````
wruser@vxros2:/work make distclean
wruser@vxros2:/work make
wruser@vxros2:/work exit
````

It could be that the build fails if it runs behind the firewall, see #22. In this case, rerun it without a certificate check as
````
wruser@vxros2:/work WGET_OPT="--no-check-certificate -O" CURL="" make
````

**Run ROS 2 examples**
````
-> ls "/usr"
/usr/bin
/usr/lib
/usr/share
/usr/vxscript
````

**Raspberry Pi 4**

Follow README to deploy VxWorks on the SDCard. Copy the content of the deploy directory to the '/usr' directory of the SDCard
````

$ sudo cp -r -L ./output/export/deploy/* /usr/.
````

**Run ROS 2 C++ examples**

It is possible to run examples by directly invoking binaries
````
-> cmd
[vxWorks *]# /usr/lib/examples_rclcpp_minimal_timer/timer_lambda
Launching process 'timer_lambda' ...
Process 'timer_lambda' (process Id = 0xffff80000046f070) launched.
[INFO] [minimal_timer]: Hello, world!
[INFO] [minimal_timer]: Hello, world!
````
Or by using the ros2cli interface
````
-> cmd
[vxWorks *]# python3 ros2 run examples_rclcpp_minimal_timer timer_lambda
Launching process 'python3' ...
Process 'python3' (process Id = 0xffff800008268ac0) launched.
[INFO] [minimal_timer]: Hello, world!
[INFO] [minimal_timer]: Hello, world!
````

**Run ROS 2 Python examples**

It is possible to run examples by directly invoking Python scripts. First, figure out the full path.
````
[vxWorks *]# python3 ros2 pkg executables --full-path demo_nodes_py
 Launching process 'python3' ...
 Process 'python3' (process Id = 0xffff80000046f070) launched.
/usr/lib/demo_nodes_py/add_two_ints_client
/usr/lib/demo_nodes_py/add_two_ints_client_async
/usr/lib/demo_nodes_py/add_two_ints_server
/usr/lib/demo_nodes_py/listener
/usr/lib/demo_nodes_py/listener_qos
/usr/lib/demo_nodes_py/listener_serialized
/usr/lib/demo_nodes_py/talker
/usr/lib/demo_nodes_py/talker_qos
````

Then invoke the Python interpreter and pass the script as a parameter
````
[vxWorks *]# python3 /usr/lib/demo_nodes_py/talker
[INFO] [talker]: Publishing: "Hello World: 0"
[INFO] [talker]: Publishing: "Hello World: 1"
````
Or by using the ros2cli interface
````
[vxWorks *]# python3 ros2 run demo_nodes_py talker
Launching process 'python3' ...
Process 'python3' (process Id = 0xffff800008269c00) launched.

[INFO] [talker]: Publishing: "Hello World: 0"
[INFO] [talker]: Publishing: "Hello World: 1"
````
Run dummy_robot, see this tutorial for more details
On the VxWorks side
````
[vxWorks *]# python3 ros2 launch dummy_robot_bringup dummy_robot_bringup_launch.py
Launching process 'python3' ...
Start RViz on your Linux machine to see a robot arm moving, or run

~/ros2_native_ws$ ros2 topic list
/joint_states
/map
/parameter_events
/robot_description
/scan
/tf
/tf_static
`````
