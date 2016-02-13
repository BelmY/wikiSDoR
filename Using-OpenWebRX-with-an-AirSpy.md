First you should install the `airspy_rx` tool on Linux:

    sudo apt-get install build-essential cmake libusb-1.0-0-dev pkg-config
    git clone https://github.com/airspy/host airspy-host
    cd airspy-host
    mkdir build
    cd build
    cmake ..
    make
    sudo make install
    sudo ldconfig

The next step is to make the follwoing changes to `config_webrx.py`:

    samp_rate = 2500000
    center_freq = 144900000
    start_rtl_command="airspy_rx -f144.9 -r /dev/stdout -a1 | nc -vvl 127.0.0.1 8888".format(rf_gain=rf_gain, center_freq=center_freq, samp_rate=samp_rate, ppm=ppm)
    format_conversion="csdr convert_i16_f"

Please make sure that the center frequency after the `-f` switch refers to the same (in MHz) as the `center_freq` parameter (in Hz).

If you want the 10 Msps sample rate, you should:
* set the `-a0` switch on `airspy_rx` (instead of the `-a1`present now)
* also set the sampling rate accordingly: `samp_rate = 10000000`