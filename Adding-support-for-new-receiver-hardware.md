You can easily add support for other receiver hardware, if it has a commandline tool that can output I/Q samples to the standard output.

If your receiver hardware is not supported yet, you will definitely need to edit `config_webrx.py`. Take a look at these lines:

    start_rtl_command="rtl_sdr -s {samp_rate} -f {center_freq} -p {ppm} -".format(rf_gain=rf_gain, center_freq=center_freq, samp_rate=samp_rate, ppm=ppm)
    format_conversion="csdr convert_u8_f"

Notice that the command to be run by OpenWebRX is something like:

    rtl_sdr (...) -

The `rtl_sdr` tool is called with various commandline parameters, which are substituted from other settings (like center frequency, sampling rate, PPM). Then the `-` at the end says that `rtl_sdr` should output the samples **to the standard output instead of a file**.

There is another important setting: `format_conversion` will tell OpenWebRX that the RTL-SDR outputs 8-bit unsigned samples. We have to convert these to 32-bit floating point samples in order to precess them with `csdr`. The available conversion options are listed on the [csdr project page](https://github.com/simonyiszk/csdr#data-types). As a quick reference, you can use:

* `csdr convert_s16_f` for receivers that output signed 16-bit integer data type (AFEDRI SDR, Softrock, etc.),
* `csdr convert_s8_f` for receivers that output signed 8-bit integer data type (HackRF).

(To understand how and where does data go within OpenWebRX, and how does it further utilize OS pipes and FIFOs, there is a graph in the [thesis paper](http://openwebrx.org/bsc-thesis.pdf#page=29).)

## Testing

You can confirm that your receiver's commandline tool works like this:

    rtl_sdr - | xxd

It works if you see a bunch of hexadecimal data:

```
0009c50: c0a6 a71d 48a3 b711 ac24 4a3e dcbd e951  ....H....$J>...Q
0009c60: 83df 7498 24da 37e9 34e1 8f24 da82 a80f  ..t.$.7.4..$....
0009c70: 5538 b926 cefe b76e 9609 4e01 451a 01fc  U8.&...n..N.E...
```
...and it doesn't work if you do not see anything.

## Remember

...to set receiver parameters correctly. Let's say, you own a brand new product called WhateverSDR, which outputs 16-bit I/Q samples at a fixed rate of 200000 sps, from the 2-meter band (145 MHz). It has a commandline tool called `whatever_sdr`, but it doesn't let you set the sampling rate from the command line. Then you would do this:

    start_rtl_command="whatever_sdr -"
    format_conversion="csdr convert_s16_f"

Still you have to set the `samp_rate = 200000` and `center_freq = 145000000` in `config_webrx.py` so that OpenWebRX would know what to write on the frequency scale, and what to expect while processing the I/Q data.
