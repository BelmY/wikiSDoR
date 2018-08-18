The following instructions assume that you are using Ubuntu 14.04 or later.

## Install prerequisite
    sudo apt-get install build-essential git libfftw3-dev cmake libusb-1.0-0-dev

## Build and install libairspyhf from IW0HDV fork
    cd ~/
    git clone https://github.com/IW0HDV/airspyhf.git -b tools
    cd airspyhf/
    mkdir build
    cd build
    cmake ../ -DINSTALL_UDEV_RULES=ON
    make
    sudo make install
    sudo ldconfig


## Download, build and install csdr library (which is a dependency of OpenWebRX) 
    git clone https://github.com/simonyiszk/csdr.git
    cd ~/csdr
    make
    sudo make install


## Install OpenWebRx
Clone IW0HDV fork, that contains a configuration file suitable for Perseus:    
    
    cd ~/
    git clone https://github.com/IW0HDV/openwebrx.git -b airspyhf

## Run as usual

    ptyhon openwebrx.py


