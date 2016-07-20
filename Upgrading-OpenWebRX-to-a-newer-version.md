> The instructions provided here to upgrade OpenWebRX and csdr will only work if the only file you modified in openwebrx and csdr directories is `config_webrx.py`. 

When upgrading OpenWebRX, first upgrade csdr:

```bash
cd csdr
git pull 
make && sudo make install
```

Only the corresponding (latest) versions of OpenWebRX and csdr will work together.

The OpenWebRX git repository contains the default configuration file `config_webrx.py` along with the software.
In newer versions of OpenWebRX, the configuration options might change, which makes the upgrade harder to do automatically. 

The clean way to upgrade OpenWebRX is to clone the git repository again, and edit `config_webrx.py` manually:

```bash
git clone https://github.com/simonyiszk/openwebrx 
cd openwebrx
nano config_webrx.py
python2 ./openwebrx.py
```

However, there is another way: **the following sequence of commands might upgrade your current OpenWebRX instance very fast, but there is also a chance that your config file will be screwed up, so you should start by making a backup.**

```bash
cd openwebrx
git status #continue only if you did not modify anything else that config_webrx.py
cp config_webrx.py config_webrx.py.bak
git stash #this puts your changes to config_webrx.py on the "stack" of the git version control system
git pull
git stash pop #this applies your changes to the current config
```

After that, you should check your config file manually:

    less config_webrx.py

There should be **no duplicate entries** (the last one will be valid).

If git failed to merge your changes to the new config file, you should manually edit the new config file to copy the correct settings from the old one. You can find the old config file at:

    less config_webrx.py.bak