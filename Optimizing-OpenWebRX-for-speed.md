To decrease OpenWebRX CPU usage:
* in `config_webrx.py`:
 * you can set `fft_size` lower (which degrades horizontal resolution of the waterfall display),
 * you can set `fft_voverlap_factor` to 0,
* in `csdr.py`:
 * you can set transition bandwidths higher (this degrades filter sharpness by decreasing the length of the kernel, but also decreases CPU usage).