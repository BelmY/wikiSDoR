OpenWebRX should work out-of-the-box on your RPi, if you set it up by [going through this guide](http://ha5kfu.sch.bme.hu/openwebrx-quick-setup).

On this page I list some common errors that people have encountered:

* [Audio underruns and `csdr flowcontrol` issue](https://github.com/simonyiszk/openwebrx/issues/9)
* Port 8888 is already in use on your system (edit `config_rtl.py` and `config_webrx.py` and replace this port number with another).

## Some general advice to increase reliability and security

### Use a firewall and put `sshd` away from TCP port 22

(TBD)

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
* Run `fsck` with a keyboard and HDMI screen attached.

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

    fsck /dev/mmcblk0p2