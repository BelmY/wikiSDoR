Just use Hansi DL9RDZ's fork of libairspyhf+

[libairspyhf+ DL9RDZ fork](https://github.com/dl9rdz/airspyhf)

This version contains airspyhf_rx, which provides raw IQ data to stdout.

Since the output of airspyhf_rx is float, you don't need format conversion in OpenWebRX:

> format_conversion = ""

Also mind your start_rtl_command accordingly

> start_rtl_command = "/path/to/airspyhf_rx -f{cf} -r /dev/stdout".format(cf=cf)

and of course your samplerate would be 768000

Enjoy!

73 de Stefan DC7DS
