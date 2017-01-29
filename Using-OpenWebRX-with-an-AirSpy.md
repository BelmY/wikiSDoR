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

**For AirSpy R1/R2:**

```python
samp_rate = 2500000 #can only be 2500000 or 10000000
center_freq = 144900000
rf_gain = 8
bias_tee = 0
start_rtl_command = "airspy_rx -f{center_freq} -r /dev/stdout -a{samp_rate_switch} -g{rf_gain} -b{bias_tee}".format(bias_tee=bias_tee, rf_gain=rf_gain, center_freq=str(center_freq/1e6), samp_rate_switch=(0 if samp_rate==10000000 else 1))
format_conversion = "csdr convert_s16_f"
```

**For AirSpy Mini:**

```python
samp_rate = 3000000 
center_freq = 144900000
rf_gain = 8
bias_tee = 0
start_rtl_command = "airspy_rx -f{center_freq} -r /dev/stdout -a{samp_rate_switch} -g{rf_gain} -b{bias_tee}".format(bias_tee=bias_tee, rf_gain=rf_gain, center_freq=str(center_freq/1e6), samp_rate_switch=2)
format_conversion = "csdr convert_s16_f"
```

Note: It is likely that you will need to use the hardware at 2.5 - 3 Msps, higher bandwidth is currently not supported.