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