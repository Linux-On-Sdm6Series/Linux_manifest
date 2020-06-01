# Hybris 16.0

## Install Packages:
sudo dpkg --add-architecture i386 && sudo apt-get update && sudo apt-get upgrade -y && sudo apt-get install -y git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev libgl1-mesa-dev libxml2-utils xsltproc unzip bc repo nano && sudo apt-get install -y git gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 x11proto-core-dev libx11-dev libgl1-mesa-dev libxml2-utils xsltproc unzip bc repo nano libssl-dev && sudo apt-get install -y openjdk-8-jdk android-tools-adb bc bison build-essential curl flex g++-multilib gcc-multilib gnupg gperf imagemagick lib32readline-dev lib32z1-dev liblz4-tool libncurses5-dev libsdl1.2-dev libssl-dev libwxgtk3.0-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc yasm zip zlib1g-dev && sudo apt-get install -y build-essential kernel-package libncurses5-dev bzip2 android-liblog android-libbacktrace libtinyxml2-6 android-libutils android-sdk-build-tools git-lfs libncurses5:i386 libncurses5 ccache build-essential p7zip-full git libgtk2.0-dev chrpath libncurses5-dev libdbus-1-dev ruby libgl1-mesa-dev "^libxcb.*" libx11-xcb-dev libxrender-dev libxi-dev flex bison gperf patchelf build-essential kernel-package libncurses5-dev bzip2 android-liblog android-libbacktrace libtinyxml2-6 android-libutils android-sdk-build-tools git-lfs libncurses5:i386 libncurses5 ccache

# Create User:
## Create User :
```bash
sudo adduser $NAME_USER
```
```EXAMPLE
EX: sudo adduser Nobita
```
## Promote Root For New User:
```bash
sudo usermod -aG sudo $NAME_USER
```
```EXAMPLE
EX: sudo usermod -aG sudo Nobita
```
## Join New User:
```bash
su $NAME_USER
```
```bash
EX: su Nobita
```
# Crate Script
Create File [.hadk.env](https://google.com)
```bash
cat << EOF >> ~/.hadk.env
export PLATFORM_SDK_ROOT="/srv/mer"
export ANDROID_ROOT="$HOME/hadk"
export VENDOR="xiaomi" # Edit Branch
export DEVICE="whyred" # Edit Device
# ARCH conflicts with kernel build
export PORT_ARCH="armv7hl"
export RELEASE="3.3.0.16" # Edit New RELEASE
export EXTRA_NAME="-Nobita" # Edit Developer Builder

# If not running interactively, don't do anything else
[[ $- != *i* ]] && return

echo "$ export LANG=C LC_ALL=POSIX"
export LANG=C LC_ALL=POSIX
echo "$ cd \$ANDROID_ROOT"
cd $ANDROID_ROOT
EOF
```
Create File [.mersdk.profile](https://google.com)
```bash
cat << EOF >> ~/.mersdk.profile
PS1="PlatformSDK $PS1"
if [ -d /etc/bash_completion.d ]; then
   for i in /etc/bash_completion.d/*;
   do
      . $i
   done
fi
EOF
```
Create File [.mersdkubu.profile](https://google.com)
```bash
cat << EOF >> ~/.mersdkubu.profile
function hadk() { source $HOME/.hadk.env; echo "Env setup for $DEVICE"; }
export PS1="HABUILD_SDK [${DEVICE}] $PS1"
hadk
EOF
```
Add This To Create File [.bashrc](https://google.com)
```bash
# SailfishOS
export HISTFILE="$HOME/.bash_history"
export HISTSIZE=1000
export HISTCONTROL=ignoreboth
export PATH=$HOME/bin:$PATH
export PLATFORM_SDK_ROOT="/srv/mer"
export ANDROID_ROOT="$HOME/hadk"
export VENDOR="xiaomi"
export DEVICE="whyred"
export PORT_ARCH="armv7hl"
shopt -s histappend
alias sfossdk="$PLATFORM_SDK_ROOT/sdks/sfossdk/mer-sdk-chroot"
alias sfos_sdk="sfossdk"
alias platform_sdk="sfossdk"
alias plat_sdk="sfossdk"
alias platformsdk="sfossdk"
alias platsdk="sfossdk"
```
# Settup Sfos SDK:
## Install SDK
```bash
cd ~/
```
```bash
curl -k -O http://releases.sailfishos.org/sdk/installers/latest/Jolla-latest-SailfishOS_Platform_SDK_Chroot-i486.tar.bz2 
```
```bash
sudo mkdir -p /srv/mer/sdks/sfossdk
```
```bash
sudo tar --numeric-owner -p -xjf Jolla-latest-SailfishOS_Platform_SDK_Chroot-i486.tar.bz2 -C /srv/mer/sdks/sfossdk
```
```bash
/srv/mer/sdks/sfossdk/mer-sdk-chroot # Join To Sfos SDK
```
or:
```bash
sfossdk
```
## Update SDK
```bash
sudo ssu re 3.3.0.16 # Edit 3.3.0.16 to New Version
```
```bash
sudo zypper ref
```
```bash
sudo zypper dup
```
# Settup Ubuntu SDK
```bash
TARBALL=ubuntu-trusty-20180613-android-rootfs.tar.bz2
curl -O https://releases.sailfishos.org/ubu/$TARBALL
UBUNTU_CHROOT=$PLATFORM_SDK_ROOT/sdks/ubuntu
sudo mkdir -p $UBUNTU_CHROOT
sudo tar --numeric-owner -xjf $TARBALL -C $UBUNTU_CHROOT
```
```bash
ubu-chroot -r $PLATFORM_SDK_ROOT/sdks/ubuntu # Join To Ubuntu SDK
```
#
# Now Sfos SDK = $PLATFORM_SDK / Ubuntu SDK = $HABUILD_SDK
##
$PLATFORM_SDK
```bash
sdk-assistant create $VENDOR-$DEVICE https://releases.sailfishos.org/sdk/targets/Sailfish_OS-$RELEASE-Sailfish_SDK_Tooling-i486.tar.7z
```
$PLATFORM_SDK
```bash
sdk-assistant create $VENDOR-$DEVICE-$PORT_ARCH https://releases.sailfishos.org/sdk/targets/Sailfish_OS-$RELEASE-Sailfish_SDK_Target-$PORT_ARCH.tar.7z
```
# Repo Sync
$HABUILD_SDK
```bash
sudo mkdir -p $ANDROID_ROOT
sudo chown -R $USER $ANDROID_ROOT
cd $ANDROID_ROOT
```
```bash
mkdir ~/bin
PATH=~/bin:$PATH
```
```bash
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
```
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```
```bash
repo init -u git://github.com/mer-hybris/android.git -b hybris-16.0
```
```bash
repo sync -vf -j$(nproc --all) --force-sync --no-clone-bundle --no-tags
```


 


















