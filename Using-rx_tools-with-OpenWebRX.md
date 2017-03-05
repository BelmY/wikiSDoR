The `rx_sdr` command works with a variety of SDR harware: RTL-SDR, HackRF, SDRplay, UHD, Airspy, Red Pitaya, and audio devices. You can find a list of [currently supported devices here](https://github.com/pothosware?utf8=%E2%9C%93&q=soapy&type=&language=).

The `rx_sdr` command syntax is similar to the `rtl_sdr` command.

It will auto-detect your SDR hardware if the following tools are installed:
* the vendor provided driver and library, 
* the vendor-specific [SoapySDR wrapper library](https://github.com/pothosware?utf8=%E2%9C%93&q=soapy&type=&language=), 
* and [SoapySDR](https://github.com/pothosware/SoapySDR) itself.

## Step #1: install SoapySDR and rx_tools
```bash
#Installing SoapySDR:
git clone https://github.com/pothosware/SoapySDR
cd SoapySDR
mkdir build
cd build
cmake ..
make 
sudo make install
cd ..

#Installing rx_tools:
git clone https://github.com/rxseger/rx_tools
cd rx_tools
mkdir build
cd build
cmake ..
make 
sudo make install
cd ..
```

## Step #2: install BOTH the vendor library and its SoapySDR wrapper
* AirSpy:
 * [libairspy](https://github.com/airspy/host)
 * [SoapyAirspy](https://github.com/pothosware/SoapyAirspy)
* HackRF:
 * [libhackrf](https://github.com/mossmann/hackrf)
 * [SoapyHackRF](https://github.com/pothosware/SoapyHackRF)
* SDRplay:
 * [SDRplay Linux driver](http://www.sdrplay.com/linuxdl.php)
 * [SoapySDRPlay](https://github.com/pothosware/SoapySDRPlay)
* RTL-SDR:
 * [librtlsdr](https://github.com/keenerd/rtl-sdr)
 * [SoapyRTLSDR](https://github.com/pothosware/SoapyRTLSDR)

### Kernel module blacklisting

Some kernel modules lock the USB device and need to be disabled before the SDR device can be used. 

If the kernel modules are not properly blacklisted you can get a "device not found" error. 

#### RTL-SDR

Add the file `/etc/modprobe.d/blacklist-rtlsdr.conf` with the following content:
```
blacklist dvb_usb_rtl28xxu
```
...then reboot the computer.

#### SDRplay
Add the file `/etc/modprobe.d/blacklist-sdrplay.conf` with the following content:
```
blacklist sdr_msi3101
blacklist msi001
blacklist msi2500
```
...then reboot the computer.

## Step #3: Test the SDR device

This command should correctly identify the SDR device attached to your computer:
```bash
SoapySDRUtil --find
```

This command should be able to open the SDR device and start to read it:
```bash
rx_sdr - | csdr through > /dev/null
```
You can terminate it with <kbd>Ctrl</kbd>+<kbd>C</kbd> or <kbd>Ctrl+<kbd>\</kbd>.

## Step #4: Edit OpenWebRX configuration 
Uncomment the corresponding lines in `config_webrx.py`:
```python
start_rtl_command="rx_sdr -F CF32 -s {samp_rate} -f {center_freq} -p {ppm} -g {rf_gain} -".format(rf_gain=rf_gain, center_freq=center_freq, samp_rate=samp_rate, ppm=ppm)
format_conversion=""
```

## Step #5: Start OpenWebRX
```bash
python2 openwebrx.py
```
