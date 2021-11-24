# Download Centos 6.10 iso
http://ftp.iij.ad.jp/pub/linux/centos-vault/6.10/isos/x86_64/

You can use it in VirtualBox or install it in a pysical partition

# Docker
If you would like to try in a docker, use the below command:
```
sudo docker run -it  --env="DISPLAY"  --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw"  centos:6.9 bash
```
X11 forwarding does not work well if you directly attach to the docker. So instead, run the command
```
yum install openssh-server
service sshd restart
```
Now that sshd service is running, find its ipaddress using ```ifconfig``` command and connect to it using X11 forwarded ssh connection
```
ssh -Y <username>@<ip address obtained from ifconfig>
```

# Fix Yum
## 1st method:
**Source:** _https://www.getpagespeed.com/server-setup/how-to-fix-yum-after-centos-6-went-eol#:~:text=Use%20the%20CentOS%20Vault%20repository&text=x%2C%20you%20can%20simply%20point,ever%20be%2C%20which%20is%206.10.&text=That's%20it%2C%20everything%20should%20be,the%20file%20%2Fetc%2Fyum._
```
curl https://www.getpagespeed.com/files/centos6-eol.repo --output /etc/yum.repos.d/CentOS-Base.repo
```

## 2nd method:
**Source:** _https://chhanz.github.io/linux/2020/12/10/centos-6-yumrepo-error/
_
```
cd /var/cache/yum;
mkdir x86_64; cd x86_64; mkdir 6; cd 6;
mkdir base extras updates centos-sclo-rh centos-sclo-sclo;

echo "https://vault.centos.org/6.9/os/x86_64/" > /var/cache/yum/x86_64/6/base/mirrorlist.txt

echo "https://vault.centos.org/6.9/extras/x86_64/" > /var/cache/yum/x86_64/6/extras/mirrorlist.txt

echo "https://vault.centos.org/6.9/updates/x86_64/" > /var/cache/yum/x86_64/6/updates/mirrorlist.txt

echo "http://vault.centos.org/6.10/sclo/x86_64/rh/" > /var/cache/yum/x86_64/6/centos-sclo-rh/mirrorlist.txt

echo "http://vault.centos.org/6.10/sclo/x86_64/sclo/" > /var/cache/yum/x86_64/6/centos-sclo-sclo/mirrorlist.txt
```

# Install SUDO
```
yum install sudo
```
## Add user to sudoer list
Open sudo config using:
```
visudo
```
Here, uncomment the line that says wheel group can run all commands:
```
## Allows people in group wheel to run all commands
%wheel  ALL=(ALL)       ALL
```
## Make a new user to add to sudoer list
```
adduser user
passwd user
```
As root user, add user to sudoer group (wheel group)
```
usermod -aG wheel user
```
**Source:** _https://phoenixnap.com/kb/how-to-create-add-sudo-user-centos_

# Update Centos and install libraries
```
yum update
yum groupinstall "Development tools"
yum install gawk texinfo epel-release gcc gcc-c++ wget zlib2-devel texinfo python-devel libjpeg-turbo libjpeg-turbo-devel zlib-devel giflib  tkinter glibc libpng gcc-c++ wget openssl openssl-devel bzip2-devel libtiff.x86_64 libtiff-devel.x86_64 libtiff-static.x86_64 doxygen doxygen-doxywizard dbus gtk2-devel.x86_64 gtk2.x86_64 gtk+extra.x86_64 gtk+extra-devel.x86_64  pygtk2.x86_64  python-gtkextra.x86_64 pywebkitgtk.x86_64  webkitgtk.x86_64 webkitgtk-devel.x86_64 wxGTK.x86_64 webkitgtk-doc.x86_64 wxGTK-devel.x86_64 glibmm24.x86_64 pygtk2-devel.x86_64  wxGTK-gl.x86_64 wxGTK-media.x86_64 libnotify-devel.x86_64 dkms xorg-x11-drv-nouveau docbook2X
```
These install all the libraries required for install gtk2 (for gtk2 based app development) and python 3.8 (or higher)

# Install python3

**Source:** _https://tecadmin.net/install-python-3-6-on-centos/_
```
cd /usr/src
su
wget https://www.python.org/ftp/python/3.8.9/Python-3.8.9.tgz
tar xzf Python-3.8.9.tgz
cd Python-3.8.9
./configure --enable-optimizations --enable-shared
make altinstall
sudo ln -sfn /usr/local/bin/python3.6 /usr/bin/python3.8
```

In .bashrc file
```
vi ~/.bashrc
```
add the lines:
```
export LD_LIBRARY_PATH=/usr/local/lib/:$LD_LIBRARY_PATH
alias python3='/usr/local/bin/python3.8'
alias pip3='/usr/local/bin/pip3.8'
```
and then do: ```source ~/.bashrc```

## To fix D-Bus
su
dbus-uuidgen > /var/lib/dbus/machine-id
     
# Alt-Installing GLIBC 2.14
**Source:** _https://stackoverflow.com/questions/35616650/how-to-upgrade-glibc-from-version-2-12-to-2-14-on-centos_

Some users prefer to alt-install glibc 2.14 in centos 6. Although it is not supported directly, we can compile it using the following steps
```
su
mkdir ~/glibc_install; cd ~/glibc_install
wget http://ftp.gnu.org/gnu/glibc/glibc-2.14.tar.gz
tar zxvf glibc-2.14.tar.gz
cd glibc-2.14; mkdir build; cd build
../configure --prefix=/opt/glibc-2.14
make -j4     # you can replace -j4 by -j<number of cores available>
sudo make install
```
In ~/.bashrc
```
vi ~/.bashrc
```
add the line:
```
export LD_LIBRARY_PATH=/opt/glibc-2.14/lib:$LD_LIBRARY_PATH
```
and do: ```source ~/.bashrc```

### Code to verify glibc-2.14 version:
```
#include <stdio.h>
#include <gnu/libc-version.h>
int main (void) { puts (gnu_get_libc_version ()); return 0; }
```
