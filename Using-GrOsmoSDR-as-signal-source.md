**Step #1:** You will need to install GNU Radio:

    sudo apt-get install gnuradio gr-osmosdr

Make sure you have at least GNU Radio version 3.7.8.1.

**Step #2:** Download [osmocom_source.grc](https://gist.githubusercontent.com/ha7ilm/19d14e1394bd2e7015e6/raw/141720f8b2b6da725fbcfc8959ee4ea4547b53a8/osmocom_source.grc) to the OpenWebRX directory.

**Step #3:** Execute the following in the OpenWebRX directory:

    mkfifo /tmp/osmocom_fifo

**Step #4:** Open [osmocom_source.grc](https://gist.githubusercontent.com/ha7ilm/19d14e1394bd2e7015e6/raw/141720f8b2b6da725fbcfc8959ee4ea4547b53a8/osmocom_source.grc) in GNU Radio Companion. 

Double-click the **osmocom Source** block, and configure the receiver.

Maybe the most important field is the *Device Arguments*, which allows you to select the receiver, e.g. `rtl=0` or `hackrf=0` are valid choices among others (detailed on the *Documentation* tab).

![osmocom_source](images/osmocom_source_config.png)

**Step #5:** Apply the same configuration to `config_webrx.py`. You will have to set at least the following settings:
* `center_freq`
* `samp_rate`

You will also have to uncomment the two relevant lines (and comment out the ones for RTL-SDR):

    # >> gr-osmosdr signal source using GNU Radio (...)
    start_rtl_command="cat /tmp/osmocom_fifo"
    format_conversion=""

**Step #6:** Execute the flowgraph in GNU Radio Companion (F6 or `Run > Execute`)

> Note: next time you can just run `python osmocom_source.py` from the command-line.

**Step #7:** Run OpenWebRX:

    python openwebrx.py



 



