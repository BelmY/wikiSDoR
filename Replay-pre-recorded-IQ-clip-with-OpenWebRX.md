This will replay the I/Q clip in a loop. 

1. Record raw I/Q data with command-line tool. 

    cd openwebrx
    rtl_sdr -s250000 -f145000000 iqfile.iq

2. Edit `config_webrx.py`:

```python 
start_rtl_command="(while true; do cat iqfile.iq; done) | csdr flowcontrol {data_rate} 10 ".format(data_rate=2*samp_rate)
format_conversion="csdr convert_u8_f"
```




