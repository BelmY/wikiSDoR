## Why is the frequency scale in OpenWebRX

## If you already know the frequency offset

## Using kalibrate-rtl

### Setting up kalibrate-rtl

    git clone https://github.com/steve-m/kalibrate-rtl
    cd kalibrate-rtl
    ./bootstrap
    ./configure
    make
    sudo make install

This will install the `kal` utility to your computer.

Use the stock antenna of the tuner.

**Step #1** is to scan for GSM channels:

    kal -g30 -sGSM900 -v

