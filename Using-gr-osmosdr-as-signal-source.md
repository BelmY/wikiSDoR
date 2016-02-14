**Step #1:** You will need to install GNU Radio:

    sudo apt-get install gnuradio gr-osmosdr

Make sure you have at least GNU Radio version 3.7.8.1.

**Step #2:** Download [osmocom_source.grc](https://gist.githubusercontent.com/ha7ilm/19d14e1394bd2e7015e6/raw/141720f8b2b6da725fbcfc8959ee4ea4547b53a8/osmocom_source.grc) to the OpenWebRX directory.

**Step #3:** Execute the following in the OpenWebRX directory:

    mkfifo osmocom_fifo

**Step #4:** Open [osmocom_source.grc](https://gist.githubusercontent.com/ha7ilm/19d14e1394bd2e7015e6/raw/141720f8b2b6da725fbcfc8959ee4ea4547b53a8/osmocom_source.grc) in GNU Radio Companion. 

![osmocom_source](images/osmocom_source_config.png)


 



