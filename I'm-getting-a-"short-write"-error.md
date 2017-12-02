This is usually an error related to the RTL-SDR, not OpenWebRX.

The steps to debug are the following:

1. Unplug the RTL-SDR from the USB port, then plug it in again.

2. Run the following: `rtl_sdr - | csdr through > /dev/null`

You should see the output rate of the RTL-SDR (around 4096000 bytes/s). 

Press <kbd>Ctrl</kbd>+<kbd>C</kbd> to stop this and exit. 

```
rtl_sdr - | csdr through > /dev/null
Found 1 device(s):
  0:  Realtek, RTL2838UHIDIR, SN: 00000001

Using device 0: Generic RTL2832U OEM
Found Rafael Micro R820T tuner
[R82XX] PLL not locked!
Sampling at 2048000 S/s.
Tuned to 100000000 Hz.
Tuner gain set to automatic.
Reading samples in async mode...
through: 4096272 bytes/s, buffer #1024
through: 4096196 bytes/s, buffer #2048
through: 4096309 bytes/s, buffer #3008
^CSignal caught, exiting!

User cancel, exiting...
```

If you don't see this, or see an error message, then there is a problem with the RTL-SDR or mostly the driver is in an inconsistent state (and it wouldn't work with other SDR software either).

3. If you are still getting a "short write" error message, reboot the computer. This should reset the USB devices.

4. Repeat step #2.