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


## References

1. Detailed steps on installing the latest firmware: https://community.st.com/stm32-mcus-60/how-can-i-upgrade-my-x-nucleo-lpm01a-s-firmware-119
2. Latest Firmware: https://www.st.com/en/development-tools/stm32-lpm01-xn.html
3. Community post: https://community.st.com/stm32cubemonitor-mcus-31/lpm01a-cannot-connect-to-stm32cubemonitor-power-11568

