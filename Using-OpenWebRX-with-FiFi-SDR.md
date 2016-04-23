FiFi SDR can be used with OpenWebRX via audio card interface.

Edit `config_webrx.py`:

    start_rtl_command="arecord -D hw:1,0 -f S16_LE -r {samp_rate} -c2 -".format(samp_rate=samp_rate)
    format_conversion="csdr convert_s16_f"
    samp_rate=192000

Also set `center_freq` correctly.

You cannot control FiFi SDR from within OpenWebRX. Use the `rockprog` tool to set the receiver parameters (like center frequency). It can be found here:

http://o28.sischa.net/fifisdr/trac/wiki/De:rockprog

> This information was shared by Benjamin, HB3YIW. Thanks for that!