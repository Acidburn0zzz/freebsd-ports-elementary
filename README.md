# FreeBSD ElementaryOS development repo

This is the [FreeBSD Ports](https://cgit.freebsd.org/ports/) collection in order to make this desktop environment usable.

It is an **early state** (very unstable). Repository contains several branches:

* [3rd-party-apps](https://codeberg.org/olivierd/freebsd-ports-elementary/src/branch/3rd-party-apps) → curated list of applications, which use the Granite toolkit
* [6.1](https://codeberg.org/olivierd/freebsd-ports-elementary/src/branch/6.1) → **main branch** (requirement `releng/13.1` or higher)
* broken
* obsolete → no need anymore

## How to test

1. `pkg install git`
2. fetch [elementary-merge](https://codeberg.org/olivierd/freebsd-ports-elementary/raw/branch/master/Tools/scripts/elementary-merge) script

You need to adjust **LOCAL_REP** variable, before to run it. The ports collection must be present in your system. Then run `sh elementary-merge -h` for more details.

![System information](https://codeberg.org/olivierd/freebsd-ports-elementary/raw/branch/master/img/switchboard-plug-about.png)

## Tips

### Webcam setup

1. Install webcamd
2. Load **cuse.ko** kernel module, and edit `/etc/rc.conf`
3. Add yourself to the **webcamd** group
4. Start **webcamd(8)** demon

```
# pkg install webcamd
# kldload cuse
# sysrc kld_list+=cuse
# pw groupmod webcamd -m olivierd

# vi /etc/rc.conf → webcamd_enable="YES"

# service webcamd start
```

5. Check available cameras

```
% usbconfig
.
.
.
ugen2.3: <NC2141102N70206E30LM21 VGA Webcam> at usbus2, cfg=0 md=HOST spd=HIGH (480Mbps) pwr=ON (500mA)
```

The USB device of my webcam is **ugen2.3**, we can adjust value of `webcamd_0_flags`.

	# vi /etc/rc.conf → webcamd_0_flags="-d ugen2.3"

Now we can test, if everything is fine. Install gstreamer1-plugings-v4l2.

	# pkg install gstreamer1-plugins-v4l2 gstreamer1-plugins-ximagesrc

	% GST_V4L2_USE_LIBV4L2=1 gst-launch-1.0 v4l2src ! xvimagesink

Or `cd multimedia/elementary-camera ; make install clean`

![Screenshot of io.elementary.camera](https://codeberg.org/olivierd/freebsd-ports-elementary/raw/branch/master/img/io.elementary.camera.png)