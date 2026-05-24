# MicroController Unit

A Microcontroller unit (MCU) is a small computer on a single integrated circuit. It contains one or more processor cores along with memory and programmable input/output peripherals. Program memory in the form of NOR flash, OTP ROM, or ferroelectric RAM is also often included on the chip, as well as a small amount of RAM. Microcontrollers are designed for embedded applications, in contrast to the microprocessors used in personal computers or other general-purpose applications consisting of various discrete chips.

Unlike a Raspberry Pi, a microcontroller has no operating system. It runs one program at a time, is extremely resource constrained and has deterministic behaviour(the same code always takes the same time and energy).

## STM32F411 Nucleo-64

STMicroelectronics produces the STM32 family of microcontrollers, all based on ARM Cortex-M cores.

The F4 series represents the performance tier:  High clock speeds, Large memory and a capable core for running demanding algorithms like crypto primitives. 
<br>
The Cortex-M4 adds: A Floating Point Unit (FPU), DSP instructions (single-cycle multiply-accumulate operations). Thus well-suited for cryptographic workloads.

### Specifications

| Feature | Specification |
|---------|---------------|
| Core | ARM Cortex M4 |
| Maximum Clock Speed | 100MHz |
| Flash Memory | 512 Kb |
| SRAM | 128 Kb |
| Package | LQFP 64 Pin |
| Operating Voltage | 1.7 - 3.6 V |
| Timers | 11 (16 bit (6), 32 bit (2), Watchdog (2), SysTick (1)) |
| GPIO Pins (General Purpose I/O) | Upto 81 out of which 50 available ?? |
| Temp | -40°C to 85°C |

 <br>

<p align="center">
  <img src="/docs/01 - Microcontroller Units/Images/Board_Img.jpg" width="400"/>
  <br>
  <em>Fig 1: Nucleo-F411RE board (Nucleo-64 type) with a STM32F411RET6 microcontroller</em>
</p>


```
STM32  F   4   11  R   E   T   6
  |    |   |    |  |   |   |   |
  |    |   |    |  |   |   |   └── Temperature range (6 = -40°C to 85°C)
  |    |   |    |  |   |   └────── Package (T = LQFP)
  |    |   |    |  |   └────────── Flash size (E = 512KB)
  |    |   |    |  └────────────── Pin count (R = 64 pins)
  |    |   |    └───────────────── Specific device number
  |    |   └────────────────────── Series (4 = F4 series)
  |    └────────────────────────── Product type (F = Foundation/mainstream)
  └─────────────────────────────── STMicroelectronics 32-bit MCU
  ```


## Hardware Layout / Components

<p align="center">
  <img src="/docs/01 - Microcontroller Units/Images/Board_Layout_Top.png" width="600"/>
  <br>
  <em>Fig 2: Board Layout - Top </em>
</p>

| | |
|---|---|
| <img src="/docs/01 - Microcontroller Units/Images/Board_Layout_Top.png" width="550"><br><em>Fig 2: Board Layout - Top </em> | <img src="/docs/01 - Microcontroller Units/Images/pinDiag.jpg" width="550"><br><em>Fig 3: Arduino and ST morpho layout</em> |

| Component | Use | Reference Image |
|-----------|-----|-----------------|
| CN-1: ST-Link USB Connector | Connects board to PC | <img src="/docs/01%20-%20Microcontroller%20Units/Images/01%20CN1.jpg" width="120"> |
| CN-2: ST-LINK/Nucleo Selector Jumper | Controls whether the ST-LINK on this board programs this board's own STM32 or an external target. For dessertation, it is left default, it bridges all pins so the onboard ST-LINK programs the onboard STM32 | <img src="/docs/01%20-%20Microcontroller%20Units/Images/02%20CN2.jpg" width="120"> |
| CN-4: Serial Wire Debug | External debug interface | <img src="/docs/01 - Microcontroller Units/Images/03 CN4.jpg" width="120"> |
| U2: ST-Link Chip | The square chip at the top half of board, aka programmer/debugger chip. Used to flash code. Talks to PC via USB and programs STM32 via SWD | <img src="/docs/01 - Microcontroller Units/Images/04 U2.jpg" width="120">
| U5: STM32F411RE | Main microcontroller, with 64 pins. Has all timers, GPIO, Flash, RAM, etc | <img src="/docs/01 - Microcontroller Units/Images/05 STM MCU.jpg" width="120"> |
| JP6: IDD Jumper | Power path switch for energy measurement. When removed, all power to the STM32 flows through the LPM01A's measurement circuit | <img src="/docs/01 - Microcontroller Units/Images/06 JP6.jpg" width="120"> |
| B1: USER Button | Blue button, connected to GPIO pin PC13. Can read its state in code | <img src="/docs/01 - Microcontroller Units/Images/07 UserButton.jpg" width="120"> |
|B2: RESET Button | Black button, resets STM32. Program restarts, Flash memory is preserved | <img src="/docs/01 - Microcontroller Units/Images/08 ResetButton.jpg" width="120"> |
| LD1: Red/Green LED (COM) | ST-LINK communication indicator. Cannot be controlled |   |
| LD2: Green LED | Connected to GPIO PA5. Can be control by code |   |
| LD3: Red LED | Indicates the board has power. Always on when USB is connected |  |
| CN5, CN6, CN8, CN9:  Arduino Headers | Standard Arduino pin layout | <img src="/docs/01 - Microcontroller Units/Images/12 Arduino.jpg" width="120"> |
| CN7, CN10: ST Morpho Headers | Used when pins not available on the Arduino headers are needed. These expose every GPIO of the STM32F411RE | <img src="/docs/01 - Microcontroller Units/Images/13 StMorphos.jpg" width="120"> |
| X1: 8MHz Crystal Oscillator | Provides the external clock signal that is used by STM32 as its reference to generate its internal 100MHz clock via a PLL (Phase Locked Loop) circuit | <img src="/docs/01 - Microcontroller Units/Images/14 X1.jpg" width="120"> |
| X2: 32kHz Crystal | Powers RTC (Real Time Clock -> a low power clock that keeps time even when the main chip is in sleep mode)| <img src="/docs/01 - Microcontroller Units/Images/15 X2.jpg" width="120"> |
| SB2: 3.3V Regulator Output | Controls the 3.3V power output on the Arduino headers. Default state is on. Used by LPM01A to understand the supply voltage during measurement. | <img src="/docs/01 - Microcontroller Units/Images/16 SB2.jpg" width="120"> |


## References

1. [um1724-stm32-nucleo64-boards-mb1136-stmicroelectronics.pdf](/docs/00%20-%20References/um1724-stm32-nucleo64-boards-mb1136-stmicroelectronics.pdf)
2. Arm Cortex-M4 Processor Technical Reference Manual Revision r0p1.<br>
Available at: https://developer.arm.com/documentation/100166/0001/
3. STMicroelectronics (2024) STM32F411 Product Page.<br>
Available at: https://www.st.com/en/microcontrollers-microprocessors/stm32f411.html
4. [STMicroelectronics (2023) STM32F411xC/xE Datasheet](/docs/00%20-%20References/stm32f411re.pdf)
5. https://en.wikipedia.org/wiki/Microcontroller
6. https://en.wikipedia.org/wiki/STM32
7. https://www.st.com/en/evaluation-tools/nucleo-f411re.html