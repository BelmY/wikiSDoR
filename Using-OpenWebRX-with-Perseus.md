To use a Microtelecom Perseus, compile the `libperseus-sdr` library that contains a suitable host tool (really the test program):

```bash
 sudo apt-get install libusb-1.0-0-dev
 cd /tmp
 wget https://github.com/Microtelecom/libperseus-sdr/releases/download/v0.7.5/libperseus_sdr-0.7.5.tar.gz
 tar -zxvf libperseus_sdr-0.7.5.tar.gz
 cd libperseus_sdr-0.7.5/
 ./configure
 make
 sudo make install
 sudo ldconfig
```

The next step is to make the following changes to `config_webrx.py`:

```python
samp_rate = 192000 # can be 48000 95000 96000 125000 192000 250000 500000 1000000 1600000 2000000
center_freq = 7070000 # can be 0 - 40000000
start_rtl_command="perseustest -s {samp_rate} -f {center_freq} -p -t 1000 -a -o -".format(center_freq=center_freq, samp_rate=samp_rate)
format_conversion=""
```

`format_conversion` is left empty as the utility program outputs I/Q as 32-bit floating point numbers.