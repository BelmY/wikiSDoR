OpenWebRX should work out-of-the-box on your RPi, if you set it up by [going through this guide](http://ha5kfu.sch.bme.hu/openwebrx-quick-setup).

On this page I list some common errors that people have encountered:

## Some general advice to increase reliability and security

### Avoid possible file system corruption on poweroff

If you just pull the plug from your Raspberry without properly shutting it down, it may or may not boot the next time. 

#### 1. Shutdown the RPi with `halt` every time

Use the proper command to halt the operating system:

    sudo halt

You will see one of the LEDs blinking at a given time interval, and then both LEDs switch off.

#### 3. Start your system in read-only mode, and switch to read-write only if required.

In my article about [pi-rw](http://ha5kfu.sch.bme.hu/node/160) you will find advice on doing that.