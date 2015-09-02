## Audio underruns

There may be multiple causes of this:

### You run out of Internet uplink bandwidth

OpenWebRX typically requires 200 kbit/s uplink per client. If you run it on a home Internet connection, and multiple clients are connected, you may run out of the uplink bandwidth your ISP allows. 

Possible solutions:
  * Reduce the FFT size (`fft_size` in `config_webrx.py`), by which you can decrease the bandwidth usage to about 100 kbit/s per client.
  * Reduce the maximum allowed number of clients to a safe value, at which no lags occur, by the `max_clients` setting in `config_webrx.py`.

### You run out of CPU time

I mean, you set a quite high sample rate (multiple Msps), and the CPU cannot handle that.
Sometimes it only happens when multiple clients connect simultaneously: when the N-th connection is made, the audio starts to lag for everyone. You can also observe around 100% CPU usage at the status bar.

You can try several things:
  * Reduce the maximum allowed number of clients to a safe value, at which no lags occur, by the  `max_clients` setting in `config_webrx.py`.
  * Reduce the sampling rate of the SDR hardware (`samp_rate` in `config_webrx.py`).
  * You can also reduce the FFT size (`fft_size`), although it has less impact on CPU usage as it is calculated only once.


