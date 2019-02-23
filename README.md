# Run PULPino on a ZedBoard 

<img src="https://raw.githubusercontent.com/pulp-platform/pulpino/master/doc/datasheet/figures/pulpino_logo_inline1.png" width="400px" />

# Introduction

PULPino is an open-source single-core microcontroller system, based on 32-bit RISC-V cores developed at ETH Zurich. PULPino is configurable to use either  the RISCY or the zero-riscy core.

First to use PULPino, clone the repository:

```bash
$ git clone https://github.com/pulp-platform/pulpino.git
```

Second you need update a ips files

```bash
$ python update-ips.py
```

Now we work in fpga folder to compile and change some files. In fpga/sw edit Makefile, in the line 16 change the git clone by `git clone https://github.com/Xilinx/linux-xlnx.git`, in the u-boot section line 38 change by `git clone https://github.com/Xilinx/u-boot-xlnx.git` and finally in line 71 change by `git clone https://git.busybox.net/buildroot`

Or clone the repository:

```bash
$ git clone ...
```



## Get Started

This synthesis flow has been tested with `Vivado 2015.1` and [`Debian 9`](https://www.debian.org/CD/) (Stretch)

In fpga folder:

1. Make sure you have the Vivado toolchain and the Xilinx SDK toolchain in your PATH before continuing.

2. Set the environment variable to select which core you want to synthesize. `setenv USE_ZERO_RISCY 1` and `setenv ZERO_RV32M 1` for zero-riscy, you need install csh program. `apt install csh`

3. ```bash
   $ make all
   ```

   It is possible depending on the version of GNU / Linux that vivado shows an error about awk and gmake, if so, it is solved by doing the following:

   ```bash
   $ export LD_LIBRARY_PATH=”” awk
   $ ln -s /usr/bin/make /usr/bin/gmake
   ```

4. Prepare the SD card and the ZedBoard for booting via SD card. To prepare the card, follow the [Xilinx guide](http://www.wiki.xilinx.com/Prepare+Boot+Medium) .

5. Copy the BOOT.BIN, uImage and devicetree.dtb files to the first partition of the SD card. Those files can be found inside the `fpga/sw/sd_image` folder.

6. Extract the content of the rootfs.tar archive and put it on the second partition of the SD card. You are ready now

7. Put the SD card into the ZedBoard and boot the system. You can use minicom (`apt install minicom`) or any other terminal emulator tool to communicate withthe UART of the ZedBoard.

   ```bash
   $ minicom -D /dev/ttyACM0 -b 115200
   ```

   And if you obtain a prompt indicating `zynq-uboot>`, type in `boot` and login with root user.