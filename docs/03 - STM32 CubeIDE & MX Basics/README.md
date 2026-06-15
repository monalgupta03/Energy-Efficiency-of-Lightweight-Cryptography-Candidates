# Understanding Basics


## LED Blinking
### Code Flow

1. HAL_Init()
2. Configure system clock
3. Configure GPIO PA5 as output
4. Loop forever:
   - LED ON
   - Delay 500 ms
   - LED OFF
   - Delay 2000 ms


## 
### PA5?
STM Pins are grouped into ports (GPIOA -> Port A, GPIOB -> Port B), Each port has pins (PA0, PA1...) <br>
PA5 -> Port A, Pin 5. <br>
//how many total ports are there?? Around 6, A to F

The onboard LED (LD2) is physically connected to PA5.

High -> 3.3V on PA5  (GPIO_PIN_SET) <br>
Low -> 0V on PA5 (GND)    (GPIO_PIN_RESET)

### Configuring GPIO?
Pins can perform many functions. They can be linked to GPIO (General Purpose Input/Output), UART, etc..

In CubeMX, we set it to PA5, hence in the code generated, it has ```GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;```

### Enable GPIOA Clock?
```__HAL_RCC_GPIOA_CLK_ENABLE(); ```

### SysTick?
Like a built in stopwatch. The timer increments every 1 ms. <br>
HAL(Hardware Abstraction Layer) counts it. Used for purposes like ```Hal_Delay```

### RCC?
Reset and Clock Control.  <br>
Module that manages all clocks: CPU clock, GPIO clock, UART clock, Timer clock, etc..

```__HAL_RCC_GPIOA_CLK_ENABLE(); ``` -> like asking RCC to supply a clock to GPIOA.

### HSE?
High Speed External.  <br>
External Clock Source. On Nucleo board ST-LINK provides an 8 MHz clock.

### HSI?
High Speed Internal. <br>
A clock generated inside the chip itself. (But less accurate than HSE)

### PLL?
Frequency Multiplier.

##

Schematics...



