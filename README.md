# OnePlus3(T)-Halium-Prebuild

## what works what don't work?
  * [x] boot
  * [x] graphics
  * [x] calls
  * [x] 3/4G
  * [x] SMS (in/out)
  * [x] sound
  * [x] gps (only on arm64)
  * [x] wifi
  * [ ] Bluetooth
  * [x] Camera (camera works, but unable to take picture for now)
  * [x] Anbox
  * [x] Sensors


### For Ubuntu Touch
#### On Linux 

before the installation, you'll need to format your userdata and your cache partition to ext4. Be careful, it'll erase all your personal files (like pictures, etc...)

just use the prebuild image just as  a normal compiled image.
install it with the JBB's halium-install script [here](https://github.com/JBBgameich/halium-install)
and get a ubports rootfs [Devel](https://ci.ubports.com/job/xenial-rootfs-armhf/lastSuccessfulBuild/artifact/out/ubports-touch.rootfs-xenial-armhf.tar.gz) or [Edge](https://ci.ubports.com/job/xenial-hybris-edge-rootfs-armhf/lastSuccessfulBuild/artifact/out/ubuntu-touch-hybris-xenial-edge-armhf-rootfs.tar.gz)
```./halium-install -p ut the_rootfs_you_choose.tar.gz system.img```
```sudo fastboot flash boot halium-boot.img```

then while in TWRP
```adb shell 'touch /data/.writable_image; mkdir /a; mount /data/rootfs.img /a; echo manual | tee /a/etc/init/rsyslog.override;  touch /a/.writable_device_image; umount /a; sync'```


#### On Windows
you'll need adb and fastboot on Windows and TWRP on your device (search on Google how to install it).  
Before the installation, you'll need to format your userdata and your cache partition to ext4. Be careful, it'll erase all your personal files (like pictures, etc...).

```fastboot flash boot halium-boot.img```  
then while in TWRP  
```
adb push rootfs.img /data/  
adb push system.img /data/  
```  
NOTE: the password with pregenerated rootfs.img is "aze"


## Anbox
```
sudo apt update
sudo apt install anbox-ubuntu-touch
```
reboot the phone and then (as there is a permission issue with anbox-tool, you'll need to use this command)
```
sudo python3 /system/halium/usr/bin/anbox-tool install
```
and finally, reboot your phone twice (why twice? no idea)

### Known Issues
* The phone heat a lot while using Anbox.

### how to compile

follow http://docs.halium.org/en/latest/porting/first-steps.html

and before ```setup oneplus3```,
replace halium/devices/manifests/oneplus_oneplus3.xml
by https://gist.github.com/Vince1171/e1aa5fda8dda213f909f71ef70de0792

(I haven't pushed my work to the halium repo as it seems that I've break plasma mobile)

```
cd halium/libhybris/ && git remote add ubports https://github.com/ubports/libhybris.git && git fetch ubports && git checkout ubports/hybris-compat-layer-fixes
cd halium/ && git clone https://github.com/ubports/platform-api.git && cd platform-api && git checkout xenial
```

```source build/envsetup.sh && breakfast halium_oneplus3-userdebug && mka halium-boot systemimage hybris-boot```
