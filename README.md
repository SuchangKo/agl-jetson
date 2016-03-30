NVIDIA Jetson TK1 AGL Distro Scripts
====================================================================
These scripts aim at building an AGL distro on Jetson TK1.

AGL Home:  
https://www.automotivelinux.org/

AGL Wiki:  
https://wiki.automotivelinux.org/


Host System Preparation:
====================================================================
You need a install various packages.

exsample: Ubuntu14.04  
sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib \  
build-essential chrpath socat libsdl1.2-dev xterm make xsltproc docbook-utils \  
fop dblatex xmlto autoconf automake libtool libglib2.0-dev


Target System Preparation:
====================================================================
You need a setup to boot from SD card at Jetson TK1.

Please proceed to the official guide of section 3.
http://developer.download.nvidia.com/embedded/L4T/r21_Release_v4.0/l4t_quick_start_guide.txt

At that time, you will need to replace command "sudo ./flash.sh ${BOARD} mmcblk0p1" to "sudo ./flash.sh 1jetson-tk1 mmcblk1p1".


Jetson TK1 exsample Yocto Distro
====================================================================
cd /path/to/work

git clone https://github.com/watatuki/agl-jetson-tk1.git

repo init -u https://github.com/watatuki/agl-jetson-tk1.git -m agl-jetson-tk1.xml

repo sync

source poky/oe-init-build-env agl-k1

cp ../agl-jetson-tk1/conf/* conf/

rm -R ../meta-agl/meta-ivi-common/recipes-multimedia/gstreamer/

bitbake agl-demo-platform


Install to SD card
====================================================================
SD card will assume that it is "/dev/sdx1".

sudo umount /dev/sdx1

sudo mkfs.ext4 /dev/sdx1

sudo mkdir -p /mnt/sdcard

sudo mount /dev/sdb1 /mnt/sdcard

cd /mnt/sdcard

sudo tar xvjf /path/to/work/agl-k1/tmp/deploy/images/jetson-tk1-upstream/agl-demo-platform-jetson-tk1-upstream.tar.bz2

sudo mv etc/rcS.d/S00psplash.sh etc/rcS.d/_S00psplash.sh

sudo cp /path/to/work/agl-jetson-tk1/start_CES2016_ivi-shell.sh opt/AGL/CES2016/

sudo sync


AGL demo running Jetson TK1 board.
====================================================================
Power ON

login by root

cd /opt/AGL/CES2016

./start_CES2016_ivi-shell.sh

