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