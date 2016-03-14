You should create a script in the OpenWebRX directory to run `sdr_commander` to initialize the receiver, and then run OpenWebRX.

**start_afedri.sh:**

```
#!/bin/bash
SAMPLE_RATE=1536000
GAIN=17
FREQUENCY=7100000
./sdr_commander  -t192.168.0.8 -sn$SAMPLE_RATE -g$GAIN -q0
./sdr_commander  -t192.168.0.8 -f$FREQUENCY
sleep 1
python openwebrx.py
```

In `config_webrx.py`, you should change these settings:

```
samp_rate = 192000
center_freq = 7080000
start_rtl_command="sdr_split -O -s{samp_rate} -f{center_freq}".format(rf_gain=rf_gain, center_freq=center_freq, samp_rate=samp_rate) 
format_conversion="csdr convert_s16_f"
```

Thanks for <a href="http://www.afedri-sdr.com/index.php/downloads/category/24-source-code-directory?download=217:experimental-openwebrx-for-afedrisdr">this experimental version of OpenWebRX</a> to Alex, 4Z5LV.