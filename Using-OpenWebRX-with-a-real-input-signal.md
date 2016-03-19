A workaround for this is to convert the real signal into a complex signal, as the DDC routines currently only work for a real signal. 

In this example, we are using an audio card as a VLF SDR:

```
center_freq = 192000 / 2
samp_rate = 192000 / 4
start_rtl_command = "arecord -f S16_LE -r 192000 -c1 - | csdr through | csdr convert_s16_f | csdr shift_addition_cc -0.25 | csdr fir_decimate_cc 2 0.005"
format_conversion = ""
```



