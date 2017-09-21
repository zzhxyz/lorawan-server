# Installation to Embedded Systems

In any case you will need a POSIX compliant operating system with "enough"
resources (CPU and memory).

## Raspberry Pi

When using Raspbian you can use the standard `.deb` package available for every
[release](https://github.com/gotthardp/lorawan-server/releases).


## Multitech mLinux

First, follow the
[guidelines](http://www.multitech.net/developer/software/mlinux/mlinux-building-images/building-a-custom-linux-image/)
to setup the mLinux build environment:
```bash
git clone git://git.multitech.net/mlinux mlinux-3.x
cd mlinux-3.x
git checkout 3.3.9
ROOT_PASSWORD="root" ./setup.sh
```

Then, download the Erlang metalayer:
```bash
cd layers
git clone https://github.com/joaohf/meta-erlang.git
git clone https://github.com/gotthardp/meta-lorawan.git
```
and to `conf/bblayers.conf` add:

```bash
BBLAYERS ?= " \
    ...
    ${TOPDIR}/layers/meta-erlang \
    ${TOPDIR}/layers/meta-lorawan \
    "
```

And build the image
```bash
source env-oe.sh
bitbake mlinux-lorawan-image
```

Finally, follow the
[guidelines](http://www.multitech.net/developer/software/mlinux/using-mlinux/flashing-mlinux-firmware-for-conduit/)
to flash the mLinux Firmware:
```bash
mkdir /var/volatile/flash-upgrade
cp <uImage path here> /var/volatile/flash-upgrade/uImage.bin
cp <rootfs path here> /var/volatile/flash-upgrade/rootfs.jffs2
touch /var/volatile/do_flash_upgrade
reboot
```

Make sure your LoRa card was correctly detected by
```bash
mts-io-sysfs show lora/hw-version
```


## OpenWRT

See the [MatchX blog](https://matchx.io/community/box/5-lorawan-server-running-on-the-box).