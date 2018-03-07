1. You should download `sdr_commander`:

http://www.afedri-sdr.com/index.php/downloads/category/15-sdr-commander-command-shell-utility

2. You should create a script in the OpenWebRX directory to run `sdr_commander` to initialize the receiver.

**start_afedri.sh:**
```
#!/bin/bash
SAMPLE_RATE=192000
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
start_rtl_command="sdr_split -i192.168.0.8 -u50000 -O -s{samp_rate} -f{center_freq}".format(rf_gain=rf_gain, center_freq=center_freq, samp_rate=samp_rate) 
format_conversion="csdr convert_s16_f"
```

3. Run `start_afedri.sh` that will also start OpenWebRX:

    bash start_afedri.sh

----

The source of this information is <a href="http://www.afedri-sdr.com/index.php/downloads/category/24-source-code-directory?download=217:experimental-openwebrx-for-afedrisdr">this experimental version of OpenWebRX</a> that Alex, 4Z5LV made. (In addition, with the instructions on this page you should be able to use a recent version of OpenWebRX as well.)