If something does not work as expected, please take a look at the terminal output of OpenWebRX carefully. Sometimes it is not obvious what is causing the problem, as by running OpenWebRX a lot of external processes a spawned, which interact with the server process. Sometimes you will get the error message from one of these processes.

## csdr: function name given in argument 1 does not exist.

You will usually receive this error message if you pulled the latest OpenWebRX from git, but you did not upgrade *csdr* as well, and the old version of *csdr* is not compatible anymore.

## TODO
  * no RTL-SDR device detected
  * my receiver is not getting listed on sdr.hu