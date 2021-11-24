DOCKER:
sudo docker run -it  --env="DISPLAY"  --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw"  centos:6.9 bash

###########
https://chhanz.github.io/linux/2020/12/10/centos-6-yumrepo-error/
###########
cd /var/cache/yum
mkdir x86_64; cd x86_64; mkdir 6; cd 6
mkdir base extras updates centos-sclo-rh centos-sclo-sclo

echo "https://vault.centos.org/6.9/os/x86_64/" > /var/cache/yum/x86_64/6/base/mirrorlist.txt

echo "https://vault.centos.org/6.9/extras/x86_64/" > /var/cache/yum/x86_64/6/extras/mirrorlist.txt

echo "https://vault.centos.org/6.9/updates/x86_64/" > /var/cache/yum/x86_64/6/updates/mirrorlist.txt

echo "http://vault.centos.org/6.10/sclo/x86_64/rh/" > /var/cache/yum/x86_64/6/centos-sclo-rh/mirrorlist.txt

echo "http://vault.centos.org/6.10/sclo/x86_64/sclo/" > /var/cache/yum/x86_64/6/centos-sclo-sclo/mirrorlist.txt

yum install sudo
visudo
Here, uncomment the line that says wheel group can run all commands:
## Allows people in group wheel to run all commands
%wheel  ALL=(ALL)       ALL

adduser user
passwd user
As root user, install sudo and add user to sudoer group (wheel group)
usermod -aG wheel user

https://phoenixnap.com/kb/how-to-create-add-sudo-user-centos

su - skh
sudo yum update
sudo yum groupinstall "Development Tools"

*********************************
*********************************
Install gcc
sudo yum install gcc gcc-c++
*********************************
To install glibcCD 
https://stackoverflow.com/questions/35616650/how-to-upgrade-glibc-from-version-2-12-to-2-14-on-centos

sudo yum install gawk
sudo yum install texinfo

mkdir /opt/glibc-2.14; mkdir /opt/glibc-2.14/etc
sudo sh -c "echo '/usr/local/lib' >> /opt/glibc-2.14/etc/ld.so.conf" 
sudo sh -c "echo '/opt/lib' >> /opt/glibc-2.14/etc/ld.so.conf"

sudo yum install wget
mkdir ~/glibc_install; cd ~/glibc_install
wget http://ftp.gnu.org/gnu/glibc/glibc-2.14.tar.gz
tar zxvf glibc-2.14.tar.gz
cd glibc-2.14; mkdir build; cd build
../configure --prefix=/opt/glibc-2.14
sudo make -j4
sudo make install

In ~/.bashrc
export LD_LIBRARY_PATH=/opt/glibc-2.14/lib:$LD_LIBRARY_PATH

Code to verify glibc-2.14 version:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
#include <stdio.h>
#include <gnu/libc-version.h>
int main (void) { puts (gnu_get_libc_version ()); return 0; }
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
*********************************

To install python3
sudo yum -y -q install zlib2-devel texinfo python-devel libjpeg-turbo libjpeg-turbo-devel zlib-devel giflib  tkinter glibc libpng gcc-c++ wget openssl openssl-devel bzip2-devel libtiff.x86_64 libtiff-devel.x86_64 libtiff-static.x86_64 doxygen doxygen-doxywizard

https://tecadmin.net/install-python-3-6-on-centos/

cd /usr/src
su
wget https://www.python.org/ftp/python/3.6.10/Python-3.6.10.tgz
tar xzf Python-3.6.10.tgz
cd Python-3.6.10
./configure --enable-optimizations --enable-shared
make altinstall
sudo ln -sfn /usr/local/bin/python3.6 /usr/bin/python3.6

In .bashrc file
#export LD_LIBRARY_PATH=/usr/local/bin:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/usr/local/lib/:$LD_LIBRARY_PATH
alias python3='/usr/local/bin/python3.6'
alias pip3='/usr/local/bin/pip3.6'


To fix D-Bus
su
yum install dbus
dbus-uuidgen > /var/lib/dbus/machine-id


gtk+2 packages for centos
sudo yum install gtk2-devel.x86_64 gtk2.x86_64 gtk+extra.x86_64 gtk+extra-devel.x86_64  pygtk2.x86_64  python-gtkextra.x86_64 pywebkitgtk.x86_64  webkitgtk.x86_64 webkitgtk-devel.x86_64 wxGTK.x86_64 webkitgtk-doc.x86_64 wxGTK-devel.x86_64 glibmm24.x86_64 pygtk2-devel.x86_64  wxGTK-gl.x86_64 wxGTK-media.x86_64 libnotify-devel.x86_64        
