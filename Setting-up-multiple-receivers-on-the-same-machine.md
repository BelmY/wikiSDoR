Right now it is only possible to do if multiple instances of OpenWebRX are started simultaneously.

1. First of all, you'll have to make a copy of the `openwebrx` directory for each separate instance. You will have something like: `openwebrx-1`, `openwebrx-2`, `openwebrx-3`...

2. You will have to make the following changes to each of the receivers:
  * `web_port` should be different in `config_webrx.py` for each receiver.
  * You should adjust the `start_rtl_command` in `config_webrx.py` for each receiver. If you have multiple RTL-SDR devices connected to the same computer, you should select which device to use by adding the `-d` flag. An example:

    `start_rtl_command="rtl_sdr -d 0 -s {samp_rate} ...`
  * You should set different `iq_server_port` for each receiver in `config_webrx.py`.

3. Please note the following:
  * You can use the same *sdr.hu* key for all of your receivers.
  * Please use a different `receiver_name` for each of your receivers, otherwise only one item will appear in the list. 