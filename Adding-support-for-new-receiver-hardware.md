As OpenWebRX relies on a lot of subprocesses, it uses OS pipes / FIFOs quite often for inter-process communication.

It is quite the same while handling the receiver hardware: to acquire the I/Q samples, OpenWebRX uses the commandline tools made for controlling the hardware.

If your receiver hardware is not supported yet, you will definitely need to do is to edit `config_webrx.py`. Take a look at these lines:

    start_rtl_command="rtl_sdr -s {samp_rate} -f {center_freq} -p {ppm} - | nc -vvl 127.0.0.1 8888".format(rf_gain=rf_gain, center_freq=center_freq, samp_rate=samp_rate, ppm=ppm)
    format_conversion="csdr convert_u8_f"

Notice that the command to be run by OpenWebRX is something like:

    rtl_sdr (...) - | nc -l localhost 8888

The `rtl_sdr` tool is called with various commandline parameters, which are substituted from other settings (like center frequency, sampling rate, PPM).

Then the `-` at the end says that `rtl_sdr` should output the samples to the standard output, from where it is piped into `netcat`.

There is another important setting: `format_conversion` will tell OpenWebRX that the RTL-SDR outputs 8-bit unsigned samples. We have to convert these to 32-bit floating point samples in order to precess them with `csdr`. The available conversion options are listed on the [csdr project page](https://github.com/simonyiszk/csdr#data-types).

The `nc` (netcat) just listens on TCP port 8888. Soon the next component that distributes I/Q data (`rtl_mus` or `ncat` in the version that is under development) connects to it, and takes the I/Q data. 

To understand how and where does data go within OpenWebRX, there is a graph in the [thesis paper](http://openwebrx.org/bsc-thesis.pdf) on page 29.

## Conclusion

You can easily add support for other receiver hardware, if:
* it has a commandline tool that can output I/Q samples to the standard output,
* it has a commandline tool that can listen on a TCP port, and then send the raw stream of I/Q samples through it if someone connects.

 