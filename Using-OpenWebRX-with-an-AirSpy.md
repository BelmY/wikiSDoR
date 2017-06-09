First you should install the `airspy_rx` tool on Linux:

```bash
sudo apt-get install build-essential cmake libusb-1.0-0-dev pkg-config
git clone https://github.com/airspy/host airspy-host
cd airspy-host
mkdir build
cd build
cmake ..
make
sudo make install
sudo ldconfig
```

The next step is to make the following changes to `config_webrx.py`:

```python
samp_rate = 2500000 #can be 2500000 or 10000000 for AirSpy R1/R2, and 3000000 for AirSpy Mini
center_freq = 144900000
rf_gain = 8
bias_tee = 0
start_rtl_command = "airspy_rx -f{center_freq} -r /dev/stdout -a{samp_rate_switch} -g{rf_gain} -b{bias_tee}".format(bias_tee=bias_tee, rf_gain=rf_gain, center_freq=str(center_freq/1e6), samp_rate=samp_rate)
format_conversion = "csdr convert_s16_f"
```

Note: It is likely that you will need to use the hardware at 2.5 - 3 Msps, higher bandwidth is currently not supported.