# Troubleshooting & Debugging

## 1. X-NUCLEO-LPM01A Not Detected in STM32CubeMonitor-Power

<b> Problem: </b> The X-NUCLEO-LPM01A Power Measurement Shield was not detected by STM32CubeMonitor-Power, preventing current and energy measurements.

Tried changing hte USB cable, yet the issue persisted. 

<b> Solution </b> 

Later on one of the community pages i found someone had a similar issue and had it resolved after updating the firmware.

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



## 3. LED Staying On Forever After Button Press

<b> Problem: </b> After flashing the ASCON encryption application, pressing the blue user button (B1) caused LD2 to turn on but never turn off, the board appeared to hang indefinitely. The only recovery was a hardware reset.

<b> Root Cause </b>

The `run_ascon()` function was being called directly from inside the interrupt handler `HAL_GPIO_EXTI_Callback()`. This function contains `HAL_Delay()` calls, which rely on the SysTick timer interrupt to count milliseconds.

Since `run_ascon()` was executing inside an interrupt context, the SysTick interrupt could not preempt it (both were at the same priority level). This caused `HAL_Delay()` to wait forever, hanging the MCU at the point LD2 was turned on.

```c
// Problematic code
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin){
   if(GPIO_Pin == B1_Pin){
      // HAL_Delay() inside here will hang forever
      run_ascon(); 
   }
}
```

<b> Solution </b>

The fix was to keep the interrupt handler as short as possible, only setting a flag and move the actual work into the main loop, where `HAL_Delay()` operates normally.

```c
// In interrupt, just setting flag
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin){
    if(GPIO_Pin == B1_Pin){
      button_pressed = 1;
   }
}

// In main loop
while (1){
    if(button_pressed){
        button_pressed = 0;
        run_ascon();
    }
}
```

After this change, pressing B1 correctly triggered the encryption routine, with LD2 turning on, flashing during the 1000 encryption iterations, and turning off cleanly upon completion.



## 4. ASCON Encryption Not Triggering on Blue Button Press When Powered via X-NUCLEO-LPM01A

<b> Problem: </b> After successfully flashing the ASCON encryption application and confirming it worked over USB, the blue user button (B1) stopped responding when the board was powered through the X-NUCLEO-LPM01A PowerShield. The LD2 LED, which is used to indicate encryption activity, showed no response on button press.

<b> Observed Behaviour </b>

When powered via USB:
- Pressing B1 triggered the interrupt correctly
- LD2 turned on, flashed during encryption, then turned off

When powered via X-NUCLEO-LPM01A (JP5 set to E5V):
- LD1 blinked red (ST-LINK searching for USB host, expected with no USB connected)
- LD3 lit up red, confirming 3.3V power was present
- Pressing B1 produced no response from LD2

<b> Initial Suspicion </b>

Since LD3 was on, power was reaching the MCU. The code was also confirmed to be properly flashed prior to switching power sources. This ruled out a flashing or power issue and pointed toward either:

- The MCU being left in a halted state from a previous debug session
- The button interrupt not firing due to a PowerShield pin conflict

<b> Debugging Steps </b>

Initially the code was flashed using Debug mode in STM32CubeIDE, followed by Resume then Terminate. This was found to sometimes leave the MCU in a halted state rather than running freely. Switching to Run mode and pressing the hardware reset button after flashing resolved this for USB-powered testing.

To isolate whether the MCU was running at all under PowerShield power, a heartbeat was added to the main loop:

```c
while (1){
    HAL_GPIO_TogglePin(GPIOA, LD2_Pin);
    HAL_Delay(500);

    if(button_pressed){
        button_pressed = 0;
        run_ascon();
    }
}
```

This allowed visual confirmation of whether the MCU was executing code independently of the button.

<b> Root Cause </b>

Work in progress: 

- **NRST being held low** — if the PowerShield drives the NRST pin, the MCU may be stuck in reset despite LD3 indicating voltage is present.