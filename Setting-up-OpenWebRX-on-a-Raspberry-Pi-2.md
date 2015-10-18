OpenWebRX should work out of the box on your RPi, if you set it up by [going through this guide](http://ha5kfu.sch.bme.hu/openwebrx-quick-setup).

You will need a **Raspberry Pi 2**.

**Raspberry Pi 1 won't work** as its CPU just doesn't have enough processing capability to make it.

(`csdr` can make use of both NEON and the multiple cores on the RPi2).

## Common issues on the RPi 2

It is not sure that you will run into any of these, but anyway:

* [Audio underruns and `csdr flowcontrol` issue](https://github.com/simonyiszk/openwebrx/issues/9)
* Port 8888 is already in use on your system:
  * Edit `config_rtl.py` and `config_webrx.py` and replace this port number with another).
* The Pi doesn't start with RTL-SDR is plugged in / restarts when plugging in RTL-SDR / fails to recongize RTL-SDR: 
  * Maybe your power supply cannot provide enough current. You should try a dedicated 5V / 2A power supply [like this](http://www.adafruit.com/product/1995). I wasn't able to use the RPi from a laptop USB port, needed a dedicated PSU.

## Compromise between performance and the maximum number of clients

The Raspberry Pi 2 should be able to handle about 5-10 clients simultaneously at the sampling rate of 250 ksps.

![](http://ha5kfu.sch.bme.hu/up/levlista/top-all-proc.png)

It is quite good from an embedded ARM board, but your PC would do of course better than that.

I'm constantly working on getting OpenWebRX faster on the Pi (in the `dev` branch I've already done some manual assembly optimizations for NEON).

You will have to set the `max_clients` and the `samp_rate` correctly. 
If you allow too much clients to connect, audio will lag for all of them (will get audio underruns), as the CPU just can't make it.

The higher the sampling rate is:
* the higher bandwidth will be seen on the waterfall diagram,
* the higher the CPU usage gets (per client as well),
* the less clients can connect without lags.

You should try to open many browser windows and see at how many clients will the audio start to lag.

## Some general advice to increase reliability and security

### Use a firewall and put `sshd` away from TCP port 22

(TODO)

### Avoid possible file system corruption on power-off

If you just pull the plug from your Raspberry without properly shutting it down, it may or may not boot the next time. 

#### 1. Shutdown the RPi with `halt` every time

Use the proper command to halt the operating system:

    sudo halt

You will see one of the LEDs blinking at a given time interval, and then both LEDs switch off.

#### 2. Start your system in read-only mode, and switch to read-write only if required.

In my article about [pi-rw](http://ha5kfu.sch.bme.hu/node/160) you will find advice on doing that.

#### 3. What if my file system has already corrupted?

`fsck` is a Linux tool meant for repairing corrupted file systems.

You can:
* Run `fsck` from a laptop running Linux, having the SD card attached.
* Run `fsck` from a [Ubuntu Live CD](http://ubuntu.com). 
* Run `fsck` from the shell over the [RPi serial port](http://elinux.org/RPi_Serial_Connection) (but you will need a serial adapter).
* Run `fsck` with a keyboard and HDMI screen attached to the RPi.

You cannot however run `fsck` from `ssh` remotely, as the OS hasn't booted properly yet.

First take a look at which device corresponds to your SD card. You can list the block devices with `lsblk`:

    ha7ilm@pc ~ $ lsblk
    NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    sda           8:0    0 123,4G  0 disk 
    ├─sda1        8:1    0 120,0G  0 part /
    └─sda2        8:2    0   3,4G  0 part [SWAP]
    mmcblk0     179:0    0   7,4G  0 disk 
    ├─mmcblk0p1 179:1    0    56M  0 part /media/ha7ilm/boot
    └─mmcblk0p2 179:2    0   7,4G  0 part /media/ha7ilm/18b2d310-8421-01f9-a0e0-1001b0d00173

Over here, the corresponding device is `mmcblk0p2`. To run `fsck` on it:

    sudo fsck /dev/mmcblk0p2