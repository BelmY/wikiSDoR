Andy Pevy, G4XYW informed me that the following changes were required to make OpenWebRX work on his Odroid C2:

In the `Makefile`:

    PARAMS_NEON = -march=armv8-a+fp+simd+crc+nocrypto -mtune=cortex-a53 -fexpensive-optimizations -funsafe-math-optimizations -Wformat=0

If `netcat` is not installed or not available, in `csdr.py`, from:

    any_chain_base="nc -v 127.0.0.1 {nc_port} | "

...to:

    any_chain_base="ncat -v 127.0.0.1 {nc_port} | "
