As OpenWebRX relies on a lot of subprocesses, it uses OS pipes / FIFOs quite often for interprocess communication.

It is quite the same with communicating to the receiver hardware: OpenWebRX uses the commandline tools provided by the hardware vendor to acquire the I/Q samples.

If your receiver hardware is not supported yet, you will definitely need to do is to edit `config_webrx.py`. Take a look at these lines:

    start_rtl_command="rtl_sdr -s {samp_rate} -f {center_freq} -p {ppm} - | nc -vvl 127.0.0.1 8888".format(rf_gain=rf_gain, center_freq=center_freq, samp_rate=samp_rate, ppm=ppm)
    format_conversion="csdr convert_u8_f"

Notice that the command that OpenWebRX will run is something like:

    rtl_sdr (...) - | nc -l localhost 8888

The `rtl_sdr` tool is called with various commandline parameters, which are substituted from other settings with the same file (like center frequency, sampling rate, PPM).

Then the `-` at the end says that `rtl_sdr` should output the samples to the standard output, from where it is piped into `netcat`.

 