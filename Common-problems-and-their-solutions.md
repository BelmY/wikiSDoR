If something does not work as expected, please take a look at the terminal output of OpenWebRX carefully. Sometimes it is not obvious what is causing the problem, as by running OpenWebRX a lot of external processes are spawned, which interact with the server process. Sometimes you will get the error message from one of these processes.

### csdr: function name given in argument 1 does not exist.

You are likely to receive this error message if you pulled the latest OpenWebRX from git, but you did not upgrade *csdr* as well, and the old version of *csdr* is not compatible anymore.

The solution is to upgrade [[csdr|https://github.com/simonyiszk/csdr]].

### socket.error: [Errno 98] Address already in use

You should try to do this before restarting OpenWebRX *(only if you don't run other instances of OpenWebRX on the same computer)*:

    sudo killall csdr

### I'm getting a totally black waterfall diagram

You have *ncat* on your system, and on some reason your RTL-SDR or other I/Q input source failed and the corresponding process exited. You will see something like:

    No supported devices found.

Please check manually that your `start_rtl_command` works, e.g. try:

    rtl_sdr - > /dev/null

### usb_claim_interface error -6, Failed to open rtlsdr device #0.

You should either blacklist the `dvb_usb_rtl28xxu` kernel module, or solve it quickly (will have to repeat it on every reboot):

    sudo rmmod dvb_usb_rtl28xxu

### rtl_sdr: invalid option -- 'p'

It seems you have an old version of `rtl_sdr`, which does not have the `-p` switch for setting the frequency correction.

Recompiling `rtl-sdr` from the latest <a href="http://sdr.osmocom.org/trac/wiki/rtl-sdr">official sources</a> (or following <a href="http://ha5kfu.sch.bme.hu/openwebrx-quick-setup">this guide</a> on setup) resolves the problem.

Sometimes it also happens that you have multiple `rtl_sdr` versions installed, located at different places. For example, if you have installed `rtl_sdr` via both the Ubuntu repositories with `sudo apt-get install` and the official Git repo with `sudo make install`, it is likely that there will be two conflicting versions in `/usr/bin` and `/usr/local/bin`. The solution is to uninstall the .deb package (`sudo apt-get remove rtl-sdr`).

### My receiver is not getting listed on sdr.hu

Receiver listing is typically updated every 5 minutes, but after that, you should suspect that something is not okay.

You should check that your receiver can be reached from the public Internet.

You can do this by reading your status page via a [web proxy](https://www.google.com/?q=free+web+proxy):

    http://my_server_address:8073/status

You should also check that the IP or DNS address you have given in the `server_hostname` parameter in `config_webrx.py` corresponds to your address on the public Internet, not your LAN. You can get your public IP address with the help of [this service](http://icanhazip.com/).

### I'm only seeing one amateur radio band on the waterfall, but I want to see the whole range of my SDR receiver, from 24 MHz to 1800 MHz

Unfortunately, this cannot be done.

The bandwidth that can be continuously acquired with SDR receivers is limited by the **sampling rate** of the receiver. With RTL-SDR, the maximum bandwidth is 2.4 MHz (corrsponding to 2.4 Msps sampling rate).

If you use other similar software like gqrx, SDR#, HDSDR, etc. you can still see a maximum bandwidth of 2.4 MHz. The 24..1800 MHz range corresponds to the *center frequency*, so you will be able to see 2.4 MHz in total, centered at somewhere within this range. For example, from 144.0 MHz to 146.4 MHz. 

There are some <a href="http://www.rtl-sdr.com/spektrum-new-rtl-sdr-spectrum-analyzer-software/">software</a> that can draw a wide-band spectrum display with an RTL-SDR, but these are tuning the center frequency of the receiver continuously. If we did that, we could have a nice waterfall but could not demodulate the signals to audio.

## I want to change the center frequency from the web interface

It cannot be done now, as OpenWebRX has been developed for use by multiple users simultaneously.

If one user changed the center frequency while others were listening to something, it would screw up the reception for all others.

That's why you can enter a fixed `center_freq` in the config file `config_webrx.py`.

However, as many have already asked for this feature because they want to use their web-based receiver on their own, it is on my roadmap to implement it.

## I want to exit OpenWebRX

Pressing Ctrl+C in the terminal should terminate OpenWebRX gracefully.

> If some of the subprocesses fail to terminate, you can force them to do so, but beware that this will affect **all instances of these processes running on the system**:

    sudo killall -9 openwebrx rtl_mus csdr rtl_sdr

## I have compiled FFTW myself, but csdr fails to link to it

If you get GCC error messages like this, when attempting to compile `csdr`:

    undefined reference to `fftwf_malloc'

...then you should recompile FFTW with the `--enable-float` option given at the configure stage.
```
./configure --enable-float
make
sudo make install
```
Then compile `csdr` again.