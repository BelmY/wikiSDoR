This will replay the I/Q clip in a loop. 

1. Record raw I/Q data with command-line tool. 

```bash
cd openwebrx
rtl_sdr -s250000 -f145000000 iqfile.iq
```

2. Edit `config_webrx.py`:

```python 
samp_rate=250000
start_rtl_command="(while true; do cat iqfile.iq; done) | csdr flowcontrol {data_rate} 10 ".format(data_rate=2*samp_rate)
format_conversion="csdr convert_u8_f"
```

**Note:** You should especially take care that the *format* and the *sampling rate* set here should be the same as for your recorded I/Q file!

----

A version that also works for 16-bit I/Q files:
```python
format_conversion="csdr convert_s16_f"
start_rtl_command="(while true; do cat iqfile.iq; done) | csdr flowcontrol {data_rate} 10 ".format(data_rate=2*samp_rate*(2 if ("u16" in format_conversion) or ("s16" in format_conversion) else 1))
```



