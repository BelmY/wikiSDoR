> Here we provide an easy way to upgrade OpenWebRX and csdr, which will work only if the source is not changed.

When upgrading OpenWebRX, first upgrade csdr:

    cd csdr
    git pull 
    make && sudo make install

Only the corresponding (latest) versions of OpenWebRX and csdr will work.

The OpenWebRX git repository also contains the default config file `config_webrx.py`.
In newer versions of OpenWebRX, the configuration options might change, which makes the upgrade harder to do automatically. 

The clean way to upgrade OpenWebRX is to clone the git repository again, and edit `config_webrx.py` manually:

    git clone https://github.com/simonyiszk/openwebrx 
    cd openwebrx
    nano config_webrx.py
    python2 ./openwebrx.py

**The following sequence of commands might upgrade your current OpenWebRX instance very fast, but there is also a chance that it will screw up your config file, so you should start by making a backup.**

    cd openwebrx
    git status #continue only if you did not modify anything else that config_webrx.py
    cp config_webrx.py config_webrx.py.bak
    git stash #this puts your changes to config_webrx.py on the "stack" of the git version control system
    git pull
    git stash pop #this applies your changes to the current config

After that, you should check your config file manually:

    less config_webrx.py

If git failed to merge your changes to the config file, you should copy the correct settings from the old config file to the new. You can find the old config file at:

    less config_webrx.py.bak
