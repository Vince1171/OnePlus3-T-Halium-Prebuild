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


some command are needed in order to get a working UT device (run as root).
```
chmod 666 /dev/kgsl-3d0

cat /var/lib/lxc/android/rootfs/ueventd*.rc|grep ^/dev|sed -e 's/^\/dev\///'|awk '{printf "ACTION==\"add\", KERNEL==\"%s\", OWNER=\"%s\", GROUP=\"%s\", MODE=\"%s\"\n",$1,$3,$4,$2}' | sed -e 's/\r//' > /etc/udev/rules.d/70-oneplus3.rules

chmod 4777 /usr/lib/dbus-1.0/dbus-daemon-launch-helper
chown root:messagebus /usr/lib/dbus-1.0/dbus-daemon-launch-helper
chmod u+s /usr/lib/dbus-1.0/dbus-daemon-launch-helper

adduser --force-badname --system --home /nonexistent --no-create-home --quiet _apt

mkdir -p /etc/system-image/config.d
mkdir /lib/firmware
ln -s /system/etc/firmware/* /lib/firmware/
ln -s /firmware/image/*  /lib/firmware/
echo manual | tee /etc/init/apparmor.override

cd /tmp
wget https://ci.ubports.com/job/pulseaudio-modules-droid/job/PR-1/8/artifact/pulseaudio-modules-droid-24_11.1.76+0~20190225000127.8~1.gbp826b96_armhf.deb --no-check-certificate
dpkg -i pulseaudio-modules-droid-24_11.1.76+0~20190225000127.8~1.gbp826b96_armhf.deb
sed -i -e "s/load-module module-droid-discover voice_virtual_stream=true/load-module module-droid-card-24/" /etc/pulse/touch.pa

```

For wifi:
```
echo sta > /sys/module/wlan/parameters/fwpath
```
