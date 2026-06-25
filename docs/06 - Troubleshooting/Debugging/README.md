# Troubleshooting & Debugging

## 1. X-NUCLEO-LPM01A Not Detected in STM32CubeMonitor-Power

<b> Problem: </b> The X-NUCLEO-LPM01A Power Measurement Shield was not detected by STM32CubeMonitor-Power, preventing current and energy measurements.

Tried changing hte USb cable, yet the issue persisted. Later on one of the community page i found someone had a similar issue and had it resolved after updating the firmware.

The pre-installed firmware version was 1.0.1. I updated it to 1.0.8 (latest at the time of writing).

Steps followed:

1. Downloaded the latest LPM01A firmware package.
2. Configured the CN2 switch to boot from system memory (USB DFU mode).
3. Connected the PowerShield via USB.
4. Opened STM32CubeProgrammer and selected:
   - Interface: USB
   - Mode: DFU
5. Loaded the firmware `.hex` file.
6. Downloaded the firmware to the target device.
7. Power cycled the board and verified the updated firmware version on the LCD.

After the upgrade, the X-NUCLEO-LPM01A was detected successfully by STM32CubeMonitor-Power.


### References

1. Detailed steps on installing the latest firmware: https://community.st.com/stm32-mcus-60/how-can-i-upgrade-my-x-nucleo-lpm01a-s-firmware-119
2. Latest Firmware: https://www.st.com/en/development-tools/stm32-lpm01-xn.html
3. Community post: https://community.st.com/stm32cubemonitor-mcus-31/lpm01a-cannot-connect-to-stm32cubemonitor-power-11568

## 2. Troubleshooting ST-LINK Server Connection Issue in STM32CubeIDE

<b> Problem: </b> While attempting to run the ASCON encryption application on the STM32 Nucleo board using STM32CubeIDE, the project was successfully compiled, but the program could not be launched on the target device.

When clicking the Run button the following error message was displayed: ```ST-Link service required to launch the debug session```

The build output confirmed that the problem was not related to the ASCON implementation or C code because:

- The project compiled successfully.
- The generated .elf file was created.
- No compiler warnings or errors were reported.

The issue appeared during the connection stage between STM32CubeIDE and the STM32 target board.


<b> Root Cause Identification </b>

Performed a search via, ```where STLinkServer.exe``` and ```Get-ChildItem "C:\Program Files" -Recurse -Filter "*STLink*"```

The search did not find ```STLinkServer.exe```. This indicated that the ST-LINK server component was not installed.

<b> Solution </b>

I downloaded and installed the official STMicroelectronics ST-LINK Server package.

The windows installer ```st-stlink-server.2.1.1-1.msi``` was exectued and the server was manually started using PowerShell. the output indicated that the ST-LINK server was running and listening for connections from STM32CubeIDE.


After starting the ST-LINK server:

STM32CubeIDE was reopened. The ASCON project was launched using Debug mode. The program then executed on the STM32 board. UART communication was verified using Tera Term, This confirmed successful execution of the ASCON encryption implementation on the STM32 hardware.


### References

1. Software Module Downloaded: https://www.st.com/en/development-tools/st-link-server.html
2. ST-Link Manual: https://www.st.com/resource/en/user_manual/um2576-stlink-server-stmicroelectronics.pdf


### Note:

Ascon was compiling and running in its previous attempts. it was after the powershiled was connected (and its firmware was updated) that this issue started to occur.

Possible reasons:

<b> ST-Link connection state changed (most likely). </b> Initially, STM32CubeIDE could directly communicate with the Nucleo board through the onboard ST-Link. If something interrupted that connection, CubeIDE may switch to requiring an external ST-Link server.

Possible triggers:

- Disconnecting/reconnecting USB
- Changing jumpers on the Nucleo board
- Powering the board from another source
- Connecting the X-NUCLEO-LPM01A PowerShield

