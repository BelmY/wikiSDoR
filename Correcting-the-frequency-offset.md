## If you already know the frequency offset

TBD

## Using kalibrate-rtl

### Setting up kalibrate-rtl

    git clone https://github.com/steve-m/kalibrate-rtl
    cd kalibrate-rtl
    ./bootstrap
    ./configure
    make
    sudo make install

This will install the `kal` utility to your computer.

**Step #1** is to scan for GSM channels:

    kal -g30 -sGSM900 -v

**Note #1:** you may have to set the band (`-s`) regarding your location, country. Available bands: `GSM850, GSM-R, GSM900, EGSM, DCS, PCS`

**Note #2:** you can use the stock antenna of the tuner.

The output of this command should look like this:

    $ kal -g30 -sGSM900  -v
    Found 1 device(s):
      0:  Generic RTL2832U OEM
    
    Using device 0: Generic RTL2832U OEM
    Found Rafael Micro R820T tuner
    Exact sample rate is: 270833.002142 Hz
    Setting gain: 30.0 dB
    kal: Scanning for GSM-900 base stations.
    channel detect threshold: 177539.149349
    GSM-900:
    	chan: 17 (938.4MHz - 39.314kHz)	power: 206788.59
    	chan: 19 (938.8MHz - 20.338kHz)	power: 370862.81
    	chan: 23 (939.6MHz - 38.748kHz)	power: 655194.63
    	chan: 27 (940.4MHz - 39.563kHz)	power: 433641.10
    	chan: 31 (941.2MHz - 39.213kHz)	power: 787826.26
    	chan: 36 (942.2MHz + 25.174kHz)	power: 474684.41
    	chan: 60 (947.0MHz + 25.120kHz)	power: 298079.65
    	chan: 83 (951.6MHz - 24.438kHz)	power: 430946.94
    	chan: 112 (957.4MHz + 24.397kHz) power: 857829.53
    
**Step #2** is to select the most powerful channel, and use it to calculate the PPM value: 
> chan: **112** (957.4MHz + 24.397kHz) power: **857829.53**

...so we use channel 112:

    kal -g30 -c112 -v

The output of this command should look like this:

    $ kal -g30 -c112 -v
    Found 1 device(s):
      0:  Generic RTL2832U OEM
    
    Using device 0: Generic RTL2832U OEM
    Found Rafael Micro R820T tuner
    Exact sample rate is: 270833.002142 Hz
    Setting gain: 30.0 dB
    kal: Calculating clock frequency offset.
    Using GSM-900 channel 112 (957.4MHz)
    	offset   1: 24292.79
    	offset   2: 24233.90
    	offset   3: 24300.03
    	offset   4: 24252.50
    	offset   5: 24212.21
    	offset   6: 24287.63
    	offset   7: 24237.00
        (...)
    	offset  96: 24244.24
    	offset  97: 24327.92
    	offset  98: 24260.77
    	offset  99: 24314.49
    	offset 100: 24262.83
    average		[min, max]	(range, stddev)
    + 24.268kHz		[24218, 24319]	(100, 28.573065)
    overruns: 0
    not found: 0
    average absolute error: -25.348 ppm
