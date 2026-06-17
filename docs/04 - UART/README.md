# Universal Asynchronous Receiver Transmitter (UART)

USART = UART + synchronous capability

A hardware communication protocol used for exchanging data between two devices. It converts binary data into electrical signals. <br>
It is asynchronous, serial (one bit by bit), point to point.

<b>Asynchronous</b>: Has no clock wire, instead has TX, RX, GND. both the devices agree beforehand on common bits/sec called <b>baud rate</b>. Garbage characters appear when STM32 baud ≠ Terminal baud.

UART message structure: ``` Idle -> Start Bit -> Data -> Parity -> Stop Bit ```

Eg: 115200 baud, <b>8</b> data bits, <b>N</b>o parity, <b>1</b> stop bit is called 8N1


## for dissertation

Via CubeMX,

```PA2 -> USART2_TX (STM32 sends data)```
```PA3 -> USART2_RX (STM32 receives data)```

have set parameters as, 115200 8N1, meaning,

|Baud Rate|115200|
|Word Length|8 bits|
|Parity| None|
|Stop Bits|1|
|Mode|TX/RX|
|Hardware Flow Control|None|

CubeMX created ```UART_HandleTypeDef huart2;``` <br>
This is storing UART configuration. eg, ```huart2.Init.BaudRate = 115200;```

