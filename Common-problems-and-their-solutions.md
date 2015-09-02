If something does not work as expected, please take a look at the terminal output of OpenWebRX carefully. Sometimes it is not obvious what is causing the problem, as by running OpenWebRX a lot of external processes a spawned, which interact with the server process. Sometimes you will get the error message from one of these processes.

### csdr: function name given in argument 1 does not exist.

You will usually receive this error message if you pulled the latest OpenWebRX from git, but you did not upgrade *csdr* as well, and the old version of *csdr* is not compatible anymore.

The solution is to upgrade [[csdr|https://github.com/simonyiszk/csdr]].

### socket.error: [Errno 98] Address already in use

You should try to do this before restarting OpenWebRX:

    sudo killall csdr

### I'm getting a totally black waterfall diagram

Xou have *ncat* on your system, and on some reason your RTL-SDR or other I/Q input source failed and the corresponding process exited. You will see something like:

    No supported devices found.

Please check manually that your `start_rtl_command` works, e.g. try:

    rtl_sdr - > /dev/null

### usb_claim_interface error -6, Failed to open rtlsdr device #0.

You should either blacklist the `dvb_usb_rtl28xxu` kernel module, or solve it quickly (will have to repeat it on every reboot):

    sudo rmmod dvb_usb_rtl28xxu

### My receiver is not getting listed on sdr.hu

You should check that your receiver can be reached from the public Internet.

You can do this by reading your status page via a [web proxy](https://www.google.com/?q=free+web+proxy):

    http://my_server_address:8073/status

You should also check that the IP or DNS address you have given in the `server_hostname` parameter in `config_webrx.py` corresponds to your address on the public Internet, not your LAN. You can get your public IP address with the help of [this service](http://icanhazip.com/).