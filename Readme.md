ZynqMP-FPGA-Linux Example (3) for UltraZed
===========================================

ZynqMP-FPGA-Linux Example (3) binary and test code for UltraZed-EG-IOCC

# Requirement

 * Board: UltraZed-EG-IOCC
 * OS: ZynqMP-FPGA-Linux ([https://github.com/ikwzm/ZynqMP-FPGA-Linux](https://github.com/ikwzm/ZynqMP-FPGA-Linux)) v2018.2


# Usage

## Boot UltraZed-EG-IOCC and login fpga user

fpga'password is "fpga".

```console
debian-fpga login: fpga
Password:
fpga@debian-fpga:~$
```

## Download this repository

### Download this repository

```console
fpga@debian-fpga:~$ mkdir examples
fpga@debian-fpga:~$ cd examples
fpga@debian-fpga:~/examples$ git clone https://github.com/ikwzm/ZynqMP-FPGA-Linux-Example-3-UltraZed negative2
fpga@debian-fpga:~/examples$ cd negative2
```

## Setup

### Copy boot.bin to boot partition and reboot

```console
fpga@debian-fpga:~/examples/negative2$ sudo cp boot/boot.bin /mnt/boot/
fpga@debian-fpga:~/examples/negative2$ sudo reboot
```

### Copy FPGA Binary file to /lib/firmware

```console
fpga@debian-fpga:~/examples/negative2$ sudo cp negative2.bin /lib/firmware
```

### Configuration FPGA with Device Tree Overlay

```console
fpga@debian-fpga:~/examples/negative2$ dtc -I dts -O dtb -o fpga-load.dtb fpga-load.dts
fpga@debian-fpga:~/examples/negative2$ sudo mkdir /config/device-tree/overlays/fpga
fpga@debian-fpga:~/examples/negative2$ sudo cp fpga-load.dtb /config/device-tree/overlays/fpga/dtbo
[   56.218477] fpga_manager fpga0: writing negative2.bin to Xilinx ZynqMP FPGA Manager
```

### Configuraiton PL Clock 0

```console
fpga@debian-fpga:~/examples/negative2$ dtc -I dts -O dtb -o fclk0-zynqmp.dtb fclk0-zynqmp.dts
fpga@debian-fpga:~/examples/negative2$ sudo mkdir /config/device-tree/overlays/fclk0
fpga@debian-fpga:~/examples/negative2$ sudo cp fclk0-zynqmp.dtb /config/device-tree/overlays/fclk0/dtbo
[  111.238976] fclkcfg amba:fclk0: driver installed.
[  111.243617] fclkcfg amba:fclk0: device name    : fclk0
[  111.248737] fclkcfg amba:fclk0: clock  name    : pl0
[  111.253678] fclkcfg amba:fclk0: clock  rate    : 99999999
[  111.259085] fclkcfg amba:fclk0: clock  enabled : 1
[  111.263833] fclkcfg amba:fclk0: remove rate    : 1000000
[  111.269125] fclkcfg amba:fclk0: remove enable  : 0
```

### Install Uio and Udmabuf Device Tree

```console
fpga@debian-fpga:~/examples/negative2$ dtc -I dts -O dtb -o negative2.dtb negative2.dts
fpga@debian-fpga:~/examples/negative2$ sudo mkdir /config/device-tree/overlays/negative2
fpga@debian-fpga:~/examples/negative2$ sudo cp negative2.dtb /config/device-tree/overlays/negative2/dtbo
[  164.123998] udmabuf amba_pl@0:negative-udmabuf4: driver probe start.
[  164.131871] udmabuf udmabuf4: driver installed
[  164.136254] udmabuf udmabuf4: major number   = 244
[  164.141021] udmabuf udmabuf4: minor number   = 0
[  164.145619] udmabuf udmabuf4: phys address   = 0x0000000070400000
[  164.151689] udmabuf udmabuf4: buffer size    = 1048576
[  164.156811] udmabuf udmabuf4: dma coherent   = 0
[  164.161412] udmabuf amba_pl@0:negative-udmabuf4: driver installed.
[  164.167998] udmabuf amba_pl@0:negative-udmabuf5: driver probe start.
[  164.175758] udmabuf udmabuf5: driver installed
[  164.180142] udmabuf udmabuf5: major number   = 244
[  164.184917] udmabuf udmabuf5: minor number   = 1
[  164.189505] udmabuf udmabuf5: phys address   = 0x0000000070500000
[  164.195576] udmabuf udmabuf5: buffer size    = 1048576
[  164.200698] udmabuf udmabuf5: dma coherent   = 0
[  164.205298] udmabuf amba_pl@0:negative-udmabuf5: driver installed.
```

## Run negative2.py

```console
fpga@debian-fpga:~/examples/negative$ sudo python3 negative.py
total:9.228[msec] setup:0.803[msec] xfer:7.894[msec] cleanup:0.532[msec]
total:1.093[msec] setup:0.618[msec] xfer:0.020[msec] cleanup:0.455[msec]
total:1.071[msec] setup:0.597[msec] xfer:0.020[msec] cleanup:0.454[msec]
total:1.071[msec] setup:0.599[msec] xfer:0.019[msec] cleanup:0.453[msec]
total:1.083[msec] setup:0.596[msec] xfer:0.020[msec] cleanup:0.468[msec]
total:1.071[msec] setup:0.600[msec] xfer:0.020[msec] cleanup:0.452[msec]
total:1.069[msec] setup:0.596[msec] xfer:0.020[msec] cleanup:0.453[msec]
total:1.071[msec] setup:0.597[msec] xfer:0.020[msec] cleanup:0.454[msec]
total:8.956[msec] setup:0.608[msec] xfer:7.889[msec] cleanup:0.459[msec]
average_setup_time  :0.624[msec]
average_cleanup_time:0.464[msec]
average_xfer_time   :1.769[msec]
throughput          :148.187[MByte/sec]
np.negative(udmabuf4) == udmabuf5 : OK
```

## Clean up

```console
fpga@debian-fpga:~/examples/negative$ sudo rmdir /config/device-tree/overlays/negative2
[  749.266776] udmabuf udmabuf5: driver uninstalled
[  749.271568] udmabuf amba_pl@0:negative-udmabuf5: driver unloaded
[  749.277680] udmabuf udmabuf4: driver uninstalled
[  749.282536] udmabuf amba_pl@0:negative-udmabuf4: driver unloaded
fpga@debian-fpga:~/examples/negative$ sudo rmdir /config/device-tree/overlays/fclk0
[  760.491074] fclkcfg amba:fclk0: change rate    : 992064
[  760.496344] fclkcfg amba:fclk0: change enable  : 0
[  760.501388] fclkcfg amba:fclk0: driver unloaded
fpga@debian-fpga:~/examples/negative$ sudo rmdir /config/device-tree/overlays/fpga
```

# Build Bitstream file

## Requirement

* Vivado 2017.2
* Vivado-HLS 2017.2

## Download this repository

```console
shell$ git clone https://github.com/ikwzm/ZynqMP-FPGA-Linux-Example-3-UltraZed 
shell$ cd ZynqMP-FPGA-Linux-Example-3-UltraZed 
shell$ git submodule init
shell$ git submodule update
```

## Run Vivado HLS

```console
vivado% cd hls
vivado% vivado_hls -f run_hls.tcl
```

## Create Vivado Project

```console
vivado% cd project
vivado% vivado -mode batch -source create_project.tcl
```

## Build Bitstream file

```console
vivado% cd project
vivado% vivado -mode batch -source implementation.tcl
vivado% cp project.runs/impl_1/design_1_wrapper.bit ../negative2.bit
```

## Convert to Binary file from Bitstream file

```console
vivado% bootgen -image negative2.bif -arch zynqmp -w -o negative2.bin
```

# Build boot.bin

## Download this repository

```console
shell$ git clone https://github.com/ikwzm/ZynqMP-FPGA-Linux-Example-3-UltraZed 
shell$ cd ZynqMP-FPGA-Linux-Example-3-UltraZed/boot/
```

## Download ZynqMP-FPGA-Linux/target/UltraZed-EG-IOCC/build-v2018.2

```console
shell$ git clone https://github.com/ikwzm/ZynqMP-FPGA
shell$ cp ZynqMP-FPGA-Linux/target/UltraZed-EG-IOCC/build-v2018.2/*.elf .
```

## Build boot.bin

```console
vivado% bootgen -arch zynqmp -image boot.bif -w -o boot.bin
```

