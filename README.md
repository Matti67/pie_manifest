# pie_manifest

Getting Android-x86 source code

Firstly, refer to the AOSP page "Establishing a Build Environment" to configure your build environment. For Ubuntu 18.04, install the following required packages:
sudo apt -y install git gcc curl make repo xmllint flex m4
sudo apt -y install openjdk-8-jdk lib32stdc++6 libelf-dev
sudo apt -y install libssl-dev python-mako syslinux-utils

Then pull down the Android-x86 source tree to your working directory by:
mkdir android-x86
cd android-x86
repo init -u git://git.osdn.net/gitroot/android-x86/manifest -b $branch
repo sync --no-tags --no-clone-bundle

Where $branch is one of the branch names described in the previous section. Note the projects created or modified by android-x86 are fetched from our git server. All the other projects are still downloaded from the repositories of AOSP.

If you have issues to sync from the git protocol, try the alternative http one
repo init -u http://scm.osdn.net/gitroot/android-x86/manifest -b $branch
repo sync --no-tags --no-clone-bundle 

Using lunch commnd (recommended)

You can source the file build/envsetup.sh into your bash environment to get some shell functions to help the building:
source build/envsetup.sh

Now you can select a target by lunch command:
lunch $TARGET_PRODUCT-$TARGET_BUILD_VARIANT

where $TARGET_PRODUCT is any target described in the previous section, and possible values of $TARGET_BUILD_VARIANT are eng, user, userdebug. For example,
lunch android_x86_64-userdebug

Then you can build by m command:
m -jX iso_img

m command is equivalent to make, but you can use it in any subdirectory of the android-x86 tree. Replace X by the number of processors you have. For example, if you have a quad core CPU, replace X with 4:
m -j4 iso_img

Since froyo-x86, we also add menu selection to lunch command. Just type lunch, and you will get a list of available targets. Choose a target by inputting its number. Alternatively, just type lunch $number.

Testing

The generated image is located at
out/target/product/$TARGET_PRODUCT/$TARGET_PRODUCT.iso

You can easily test the iso file by a virtual box or qemu. On the booting screen, select the VESA or debug mode to boot.

Of course you can burn the iso to a CD disk and test it on a real hardware. On booting it will automatically detect your hardware and load necessary modules. If you have problem with the default frame buffer driver, you may try the VESA mode (select second item on boot screen).

Since honeycomb-x86, we supports the hybrid iso format. That is, the iso could be dumped to a usb disk directly. You may create a bootable USB disk by
dd if=out/target/product/x86/android_x86.iso of=/dev/sdX

where /dev/sdX is the device name of your USB disk. The feature is available for all iso files released after 2011/12/25.

For more details about how to use the iso or install it, see the installation howto.

