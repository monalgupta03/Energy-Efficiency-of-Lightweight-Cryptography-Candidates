# Universal Asynchronous Receiver Transmitter (UART)

USART = UART + synchronous capability

A hardware communication protocol used for exchanging data between two devices. It converts binary data into electrical signals. <br>
It is asynchronous, serial (one bit by bit), point to point.

<b>Asynchronous</b>: Has no clock wire, instead has TX, RX, GND. both the devices agree beforehand on common bits/sec called <b>baud rate</b>. Garbage characters appear when STM32 baud ≠ Terminal baud.

UART message structure: ``` Idle -> Start Bit -> Data -> Parity -> Stop Bit ```

Idle | Start | D0 | D1 | D2 | D3 | D4 | D5 | D6 | D7 | Stop |
  1  |   0   |                Data Bits               |   1  |

Eg: 115200 baud, <b>8</b> data bits, <b>N</b>o parity, <b>1</b> stop bit is called 8N1


## for dissertation

Via CubeMX,

- PA2 → USART2_TX (STM32 transmits data)
- PA3 → USART2_RX (STM32 receives data)

![USART Configuration](/docs/04%20-%20UART/images/PinConfiguraton.jpg)

have set parameters as, 115200 8N1, meaning,

| Parameter | Value |
|-----------|-------|
|Baud Rate|115200|
|Word Length|8 bits|
|Parity| None|
|Stop Bits|1|
|Mode|TX/RX|
|Hardware Flow Control|None|

CubeMX created ```UART_HandleTypeDef huart2;``` <br>
This is storing UART configuration. eg, ```huart2.Init.BaudRate = 115200;```

UART was selected because it provides a simple serial communication channel between the STM32 microcontroller and a PC terminal application without requiring additional hardware protocols.

The interface was used to transmit debugging messages and verify program
execution during development.

### Tera Term Setup

1. Select Serial
2. Choose COM5
3. Set (from windows):
   - Baud Rate = 115200
   - Data = 8 bit
   - Parity = None
   - Stop = 1 bit

<p align="center">
  <img src="/docs/04 - UART/images/TeraTerm.jpg" width="500"/>
  <br>
  <em>Fig 2: Tera Term Output </em>
</p>


## Issues

### Garbage Characters

Cause: STM32 baud rate did not match terminal baud rate.

Fix: Ensure both are configured to 115200 baud.

### No Output

Possible causes:
- Wrong COM port selected
- TX pin misconfigured
- UART peripheral not initialized
- Terminal settings incorrect


### Alternative Terminal Software

Initially tried PuTTY for the serial terminal application. However, no UART output was observed despite using the same serial settings. Therefore, shifted to TeraTerm for all subsequent UART verification and debugging.


Note: The NUCLEO board's onboard ST-Link debugger provides a Virtual COM Port (VCP). When connected via USB, Windows detects this as a serial device (COM5 in this setup), allowing UART messages to be viewed using a terminal application.