# FraaRTOS
This is my first tentative to create a small OS for ARM Cortex M4

# Structure 

The repo is divided in the three main parts:
* OS files (Src and Inc) 
* The testbench. This part contains some small projects for an STM32 DISCOVERY board, which I use to test the OS. There is also a primitive structure for automated testing. The top makefile can call all the make test which compile, flash and automate the GDB session. GDB outputs in gdb.output. A user written python script is then called to assess that  gdb log is fine. 
* The Doc folder which is about how to use the library and some design choices. 


# Testbench

In order to compile the testbench, aside from the hardware (discovery board and micro usb cable), you will need:

* ARM toolchain: https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads
 add to PATH the bin dir: gcc-arm-none-eabi-9-2019-q4-major/bin

* st-link: used to flash the micro and to remote debug: https://github.com/texane/stlink
  libraries required to compile it:
	* Debian based distros (debian, ubuntu)
	* build-essential
	* cmake
	* libusb-1.0 (plus development headers for building, on debian based distros libusb-1.0-0-dev package)
	* (optional) for stlink-gui we need libgtk-3-dev

* (Optional) To create a new testbench the STMCubeMX application is very useful and also creates a makefile for creating the .elf executable right out of the box

# Useful commands

To flash the image
```
sudo ./st-flash --format ihex write ../projects/blink_test/build/blink_test.hex
```

To run the remote debugger thanks to the stlink already mounted on the discovery board run 
```
sudo ./st-utils & #from the st-link folder
arm-none-eabi-gdb -x gdb_script #from the testbench folder
```
In the gdb script you can write some init commands like reading the symbol table insead of doing them every time and set some breakpoints.

To see the disassemble you can use objump and to check symbols you can use either nm, readelf or objdump again
```
arm-none-eabi-objdump -d build/fraaRTOS.o #disassemble
arm-none-eabi-objdump -x build/fraaRTOS.o #see symbols
```
