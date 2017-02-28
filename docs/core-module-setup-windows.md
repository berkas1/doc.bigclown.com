# BigClown development on Windows


<!-- toc -->


## Introduction


Our software stack and SDK can be easily run also on Windows with our IDE.
With few simple steps you can develop and test firmware, MQTT messages and more on Windows.
This tutorial was tested on Windows 7 Professional x64.


## How to download IDE


[Download BigClown IDE](https://github.com/bigclownlabs/bc-windows-ide/releases). This installer will take care of installing the following:

* Visual Studio Code IDE
* GIT command line utility to automatically get the latest SDK
* ARM-GCC compiler for Windows
* dfu-util for flashing firmware over USB


## How to compile SDK


Run the BigClown IDE. There will be opened simple code skeleton.
Now you can compile the code by pressing `Ctrl + Shift + B`.

The downloaded GitHub firmware has empty user function. Please see the tutorials and add you application logic in `app/application.c`.

Congratulation for your first compiled firmware.


## How to upload firmware over USB


After compiling the firmware. Press `Ctrl + Shift + P` in the Visual Studio code. The cursor will jump to the top command line. Press the backspace to delete the `>` symbol. Now write `task` and it will display you all the make targets the Makefile contains. We are interested in the `dfu` task. So execute command `task dfu`.

This Device Firmware Update utility (`dfu-util`) will allow you to upload compiled binary firmware just with USB.
Please follow this help [how to setup USB driver and use DFU Util on Windows](https://doc.bigclown.com/core-module.html#on-windows-10-64-bit-desktop).

[Do not forget to set the device to the boot mode](https://doc.bigclown.com/core-module.html#programming-using-usb-dfu-bootloader).

If you use command line then set the device to DFU mode and flash it by typing `make dfu`.


## How to debug core module


To debug the running code on Core Module you can use Ozone debugger with J-Link debug probe. It is also possible to use GDB/OpenOCD with other debug probes but this is not documented yet.

Download the [Ozone debugger here](https://www.segger.com/downloads/jlink#Ozone).
Ozone folder also needs to be set in PATH environment variable or you can simply edit Makefile and set absolute path to the Ozone.exe file.

You start debugging by typing `make ozone` in the command line or `Ctrl+Shift+P` and typing `task ozone` in the Visual Studio Code.


## How to generate API documentation


Our SDK is using Doxygen to automaticaly generate API documentation from C header files.
This tool is not necessary to install.
The generated API documentation is also on http://api.bigclown.com.
Download the [latest Doxygen here](http://www.stack.nl/~dimitri/doxygen/download.html).

You generate the docs by `make doc`.


## How to install virtual COM port driver


Download and install the [STM32 Virtual COM Port driver](https://drive.google.com/open?id=0B5pXL_JAACMvczQ0MVM1eUZILXc). You can also download latest driver [directly from ST](http://www.st.com/en/development-tools/stsw-stm32102.html) but you will need to register.


## How to install Mosquitto MQTT broker


[Mosquitto MQTT Broker](https://mosquitto.org/download/) is a MQTT server which is a central message hub in our system. Please download, install and run this broker. This broker needs to be running in the background all the time you use our Python Gateway.

To run the MQTT broker go to install directory and type `mosquitto.exe -v`.
Parameter `-v` is for verbose, so you can see the messages.


## How to start the gateway


Download and install [Python 3](https://www.python.org/downloads/) which is needed to run the Gateway. Gateway is a small python program which connects virtual USB serial port of Core Module and MQTT broker.

Connected core module appears as a USB serial device in your computer.
The python gateway script interconnects the core module with MQTT broker.
This section explains how to set up these pieces.

Clone bc-workroom-hub

`git clone https://github.com/bigclownlabs/bc-workroom-hub.git`

**Current bc-workroom-gateway.py needs that you comment out two lines of code where we use fcntl. Will be fixed in next revision**

Then run the script with the correct COM port set.

`python gateway/bc-workroom-gateway.py -d COM2`


## Congratulations!


You have sucessfully installed SDK with gateway. Now you can explore and make new interesting projects.
