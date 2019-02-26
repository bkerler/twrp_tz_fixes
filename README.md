For TWRP:
```
mkdir android
cd ~/android
mkdir system
cd system
repo init -u git://github.com/bkerler/platform_manifest_twrp_lineageos.git -b cm-13.0
repo sync -j8
git clone https://github.com/bkerler/twrp_tz_fixes ../patches
patch -p1 -d bootable/recovery-twrp < ~/android/patches/bootable_recovery-twrp.diff
cd device/qcom/common
git checkout 1286ac9d62dc91dcf5e1e67681ed755c9cc0c725
cd ~/android/system

#for bacon
patch -p1 -d kernel/oneplus/msm8974 < ~/android/patches/kernel_oneplus_msm8974.diff
patch -p1 -d device/oneplus/bacon < ~/android/patches/device_oneplus_bacon.diff

source build/envsetup.sh
breakfast bacon

#breakfast onyx
#breakfast hammerhead

make libcryptfs_hw
make libssl
make libhardware
make recoveryimage
```

For stock kernel (preinstall):
```
sudo apt install build-essentials
mkdir android
cd ~/android
git clone https://github.com/bq/aquaris-X-Pro.git
cd kernel
git checkout tags/2.5.1_20190114-1551
patch -p1 < ../kernel_bq_msm8953.diff
cd ..
git clone https://android.googlesource.com/platform/prebuilts/gcc/linux-x86/aarch64/aarch64-linux-android-4.9
```

make_kernel.sh :
```
rm -rf KERNEL_OUT
mkdir KERNEL_OUT
make -C kernel O=../KERNEL_OUT ARCH=arm64 CROSS_COMPILE=$HOME/android/aarch64-linux-android-4.9 bardock-pro_defconfig
make -j4 O=../KERNEL_OUT/ -C kernel ARCH=arm64 CROSS_COMPILE=$HOME/android/aarch64-linux-android-4.9/bin/aarch64-linux-android-
cp KERNEL_OUT/arch/arm64/boot/Image.gz-dtb kernel
```
