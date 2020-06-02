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
## With $HABUILD_SDK to leave, just type `exit` or Ctrl+D, and you'll be back to the PLATFORM_SDK
##
$PLATFORM_SDK
```bash
sdk-assistant create $VENDOR-$DEVICE https://releases.sailfishos.org/sdk/targets/Sailfish_OS-$RELEASE-Sailfish_SDK_Tooling-i486.tar.7z
```
$PLATFORM_SDK
```bash
sdk-assistant create $VENDOR-$DEVICE-$PORT_ARCH https://releases.sailfishos.org/sdk/targets/Sailfish_OS-$RELEASE-Sailfish_SDK_Target-$PORT_ARCH.tar.7z
```
# Sync Source
## Repo Sync
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
## libhybris
$HABUILD_SDK
```bash
cd $ANDROID_ROOT/external
```
```bash
git clone --recurse-submodules https://github.com/mer-hybris/libhybris.git
```
## hybris patches
$HABUILD_SDK
```bash
hybris-patches/apply-patches.sh --mb
```
## Remove LineageOS Bootanimaton
```bash
cd $ANDROID_ROOT
```
```bash
rm -rf vendor/lineage/bootanimation
```
## fixup-mountpoints
```bash
cd $ANDROID_ROOT
```
```bash
nano hybris/hybris-boot/fixup-mountpoints
```
Edit Like This:
Example : [*Xiaomi Redmi Note 5 Pro Whyred*](https://github.com/Linux-On-Whyred/hybris-boot/commit/9dc82c7d36dbebd2b9646ab37372ac8f7bccecb3)
## Clone Device Tree
$HABUILD_SDK
Device Tree [*LineageOS*](https://github.com/LineageOS)
##
Device Local:
```bash
$ANDROID_ROOT/device/$VENDOR/$DEVICE
```
Example:
```bash
$ANDROID_ROOT/device/xiaomi/whyred
```
##
Vendor Local:
```bash
$ANDROID_ROOT/vendor/$VENDOR/$DEVICE
```
Example:
```bash
$ANDROID_ROOT/vendor/xiaomi/whyred
```
##
Kernel Local:
```bash
$ANDROID_ROOT/kernel/$VENDOR/$DEVICE
```
Example:
```bash
$ANDROID_ROOT/kernel/xiaomi/whyred
```
# Config Kernel
## mer-kernel-check
$HABUILD_SDK
```bash
hybris/mer-kernel-check/mer_verify_kernel_config local Defconfig
```
Example:
```bash
hybris/mer-kernel-check/mer_verify_kernel_config $ANDROID_ROOT/kernel/xiaomi/whyred/arch/arm64/configs/whyred-perf_defconfig
```
Then Add the Commit By Hand
##
## check-kernel-config ( Auto Config )
$HABUILD_SDK
```bash
hybris/mer-kernel-check/check-kernel-config local Defconfig
```
```bash
hybris/mer-kernel-check/check-kernel-config local Defconfig -w
```
```bash
hybris/mer-kernel-check/check-kernel-config local Defconfig -w
```
```bash
hybris/mer-kernel-check/check-kernel-config local Defconfig -w
```
Example:
```bash
hybris/mer-kernel-check/check-kernel-config $ANDROID_ROOT/kernel/xiaomi/whyred/arch/arm64/configs/whyred-perf_defconfig
```
```bash
hybris/mer-kernel-check/check-kernel-config $ANDROID_ROOT/kernel/xiaomi/whyred/arch/arm64/configs/whyred-perf_defconfig -w
```
```bash
hybris/mer-kernel-check/check-kernel-config $ANDROID_ROOT/kernel/xiaomi/whyred/arch/arm64/configs/whyred-perf_defconfig -w
```
```bash
hybris/mer-kernel-check/check-kernel-config $ANDROID_ROOT/kernel/xiaomi/whyred/arch/arm64/configs/whyred-perf_defconfig -w
```
Must Re-Enter Many Times For Full Defconfig
# Build vendor.img + hybris-boot
$HABUILD_SDK
```bash
source build/envsetup.sh
```
```bash
export USE_CCACHE=1
```
```bash
lunch lineage_$DEVICE-userdebug
```
```bash
EX:
lunch lineage_whyred-userdebug
```
```bash
mka -j$(nproc --all) hybris-hal
```
```bash
out file:
out/target/product/$DEVICE/
```
# Create HAL
## droid-hal-whyred
PLATFORM_SDK $
```bash
cd $ANDROID_ROOT
```
```bash
mkdir rpm
```
```bash
cd rpm
```
```bash
git init
```
```bash
git submodule add https://github.com/mer-hybris/droid-hal-device dhd
``` 
```bash
sed -e "s/@DEVICE@/whyred/" \
    -e "s/@VENDOR@/xiaomi/" \
    -e "s/@DEVICE_PRETTY@/Redmi Note 5/" \
    -e "s/@VENDOR_PRETTY@/Xiaomi/" \
    dhd/droid-hal-@DEVICE@.spec.template > droid-hal-whyred.spec
```
```bash
git add .
```
```bash
git commit -m "[dhd] Initial content"
```
## droid-config-whyred
$PLATFORM_SDK
```bash
cd $ANDROID_ROOT
```
```bash
mkdir -p hybris/droid-configs
```
```bash
cd hybris/droid-configs
```
```bash
git init
```
```bash
git submodule add https://github.com/mer-hybris/droid-hal-configs droid-configs-device
```
```bash
mkdir rpm
```
```bash
sed -e "s/@DEVICE@/whyred/" \
    -e "s/@VENDOR@/xiaomi/" \
    -e "s/@DEVICE_PRETTY@/Redmi Note 5/" \
    -e "s/@VENDOR_PRETTY@/Xiaomi/" \
    droid-configs-device/droid-config-@DEVICE@.spec.template > \
    rpm/droid-config-whyred.spec
```
```bash
git add .
```
```bash
git commit -m "[dcd] Initial content"
```
```bash
cd $ANDROID_ROOT
```
```bash
rpm/dhd/helpers/add_new_device.sh
```
```bash
cd hybris/droid-configs
```
```bash
COMPOSITOR_CFGS=sparse/var/lib/environment/compositor
```
```bash
mkdir -p $COMPOSITOR_CFGS
```
```bash
cat <<EOF >$COMPOSITOR_CFGS/droid-hal-device.conf
# Config for $VENDOR/$DEVICE
EGL_PLATFORM=hwcomposer
QT_QPA_PLATFORM=hwcomposer
# Determine which node is your touchscreen by checking /dev/input/event*. WRITE ALL IN ONE LINE(:
LIPSTICK_OPTIONS=-plugin evdevtouch:/dev/input/event0 -plugin evdevkeyboard:keymap=/usr/share/qt5/keymaps/droid.qmap
EOL
```
```bash
git add .
```
```bash
git commit -m "[dcd] Patterns and compositor config"
```
## droid-hal-version-whyred
$PLATFORM_SDK
```bash
cd $ANDROID_ROOT
```
```bash
mkdir -p hybris/droid-hal-version-whyred
```
```bash
cd hybris/droid-hal-version-whyred
```
```bash
git init
```
```bash
git submodule add https://github.com/mer-hybris/droid-hal-version
```
```bash
mkdir rpm
```
```bash
sed -e "s/@DEVICE@/whyred/" \
    -e "s/@VENDOR@/xiaomi/" \
    -e "s/@DEVICE_PRETTY@/Redmi Note 5/" \
    -e "s/@VENDOR_PRETTY@/Xiaomi/" \
    droid-hal-version/droid-hal-version-@DEVICE@.spec.template > \
    rpm/droid-hal-version-whyred.spec
```
```bash
git add .
```
```bash
git commit -m "[dvd] Initial content"
```
# Start Ports SailfishOS
$PLATFORM_SDK
## Build
```bash
cd $ANDROID_ROOT
```
```bash
rpm/dhd/helpers/build_packages.sh
```
```bash
rpm/dhd/helpers/build_packages.sh --mw
```
```bash
rpm/dhd/helpers/build_packages.sh -c
```
## Create *.ks File
```bash
cd $ANDROID_ROOT
HA_REPO="repo --name=adaptation-community-common-$DEVICE-$RELEASE"
HA_DEV="repo --name=adaptation-community-$DEVICE-$RELEASE"
KS="Jolla-$RELEASE-$DEVICE-$PORT_ARCH.ks"
sed \
"/$HA_REPO/i$HA_DEV --baseurl=file:\/\/$ANDROID_ROOT\/droid-local-repo\/$DEVICE" \
$ANDROID_ROOT/hybris/droid-configs/installroot/usr/share/kickstarts/$KS > $KS
```


 


















