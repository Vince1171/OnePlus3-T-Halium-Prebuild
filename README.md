# OnePlus3(T)-Halium-Prebuild

## what works what don't work?
  * [x] boot
  * [x] graphics
  * [ ] calls
  * [x] sound
  * [x] gps
  * [x] wifi
  * [ ] Bluetooth
  * [ ] Camera
  * [x] Rotation
  * [ ] anbox binder integrated, but anbox not working


### For Ubuntu Touch

just use the prebuild image just as  a normal compiled image.
install it with the JBB's halium-install script [here](https://github.com/JBBgameich/halium-install)
and get the ubports edge rootfs from [here](https://ci.ubports.com/job/xenial-rootfs-armhf/lastSuccessfulBuild/artifact/out/ubports-touch.rootfs-xenial-armhf.tar.gz)
```./halium-install -p ut ubports-touch.rootfs-xenial-edge-armhf.tar.gz system.img```
```sudo fastboot flash boot halium-boot.img```

then while in TWRP
```adb shell 'touch /data/.writable_image; mkdir /a; mount /data/rootfs.img /a; echo manual | tee /a/etc/init/rsyslog.override;  touch /a/.writable_device_image; umount /a; sync'```


some command are needed in order to get a fully working UT device (run as root).
```
chmod 666 /dev/kgsl-3d0
adduser --force-badname --system --home /nonexistent --no-create-home --quiet _apt

cd /tmp
wget https://ci.ubports.com/job/pulseaudio-modules-droid/job/PR-1/8/artifact/pulseaudio-modules-droid-24_11.1.76+0~20190225000127.8~1.gbp826b96_armhf.deb --no-check-certificate
dpkg -i pulseaudio-modules-droid-24_11.1.76+0~20190225000127.8~1.gbp826b96_armhf.deb
sed -i -e "s/load-module module-droid-discover voice_virtual_stream=true/load-module module-droid-card-24/" /etc/pulse/touch.pa

```
