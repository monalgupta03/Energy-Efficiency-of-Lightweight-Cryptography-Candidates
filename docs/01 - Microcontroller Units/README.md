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

| Component | Use | Reference Image |
|-----------|-----|-----------------|
| CN-1: ST-Link USB Connector | Connects board to PC | ![CN1](/docs/01%20-%20Microcontroller%20Units/Images/01%20CN1.jpg) |
| CN-2: ST-LINK/Nucleo Selector Jumper | Controls whether the ST-LINK on this board programs this board's own STM32 or an external target. For dessertation, it is left default, it bridges all pins so the onboard ST-LINK programs the onboard STM32 | ![CN2](/docs/01%20-%20Microcontroller%20Units/Images/02%20CN2.jpg)




## References

1. [um1724-stm32-nucleo64-boards-mb1136-stmicroelectronics.pdf](/docs/00%20-%20References/um1724-stm32-nucleo64-boards-mb1136-stmicroelectronics.pdf)
2. Arm Cortex-M4 Processor Technical Reference Manual Revision r0p1.
Available at: https://developer.arm.com/documentation/100166/0001/
3. STMicroelectronics (2024) STM32F411 Product Page.
Available at: https://www.st.com/en/microcontrollers-microprocessors/stm32f411.html
4. [STMicroelectronics (2023) STM32F411xC/xE Datasheet](/docs/00%20-%20References/stm32f411re.pdf)
5. https://en.wikipedia.org/wiki/Microcontroller
6. https://en.wikipedia.org/wiki/STM32
7. https://www.st.com/en/evaluation-tools/nucleo-f411re.html