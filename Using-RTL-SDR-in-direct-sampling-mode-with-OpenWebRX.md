First, you will have to setup <a href="https://github.com/keenerd/rtl-sdr">keenerd's fork</a> of RTL-SDR, as it has the `-D` switch for the `rtl_sdr` tool.

    git clone https://github.com/keenerd/rtl-sdr
    cd rtl-sdr/ && mkdir build && cd build
    cmake ../ -DINSTALL_UDEV_RULES=ON
    make && sudo make install && sudo ldconfig

Then, you have to add the `-D` switch to `config_webrx.py`.

    start_rtl_command="rtl_sdr -D1 -s {samp_rate} -f {center_freq} -p {ppm} ...
                                /\___ this is the switch to add

`-D1` means that the dongle is set to direct-sampling from input 1 / I.<br />
`-D2` means that the dongle is set to direct-sampling from input 2 / Q.<br />
`-D3` means that the dongle is set to no-mod direct-sampling.
