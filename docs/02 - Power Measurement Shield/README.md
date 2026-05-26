
# Power Consumption Measurement Board (X-NUCLEO-LPM01A)

The X-NUCLEO-LPM01A is a low-power measurement consumption measurement board from STMicroelectronics designed for inline energy profiling of STM32 microcontrollers. LPM stands for Low Power Measurement.

It connects directly to Nucleo boards such as the NUCLEO-F411RE via Arduino-compatible headers and measures current, voltage and energy consumption at runtime.

The board supports high-resolution, software-controlled power monitoring and is widely used in embedded benchmarking workflows aligned with standardised energy evaluation methodologies such as those defined by EEMBC.


## Why Not Just Use a USB Cable?
Because USB carries data, wherxeas the Arduino headers carry power. <br>
The LPM01A must physically intercept the power path between the supply and the STM32. It cannot do this over USB. The measurement works by sitting inline with the current flow.

## Plysics behind the Energy Mesurement
the LPM01A measures current (Amp) via Shunt Resistor, a small precision resistor placed in series with the power line. When current flows through a resistor, it creates a small voltage drop across it. The LPM01A measures this tiny voltage drop and calculates the current from it.

```
Current (I) = V / R (Ohm's Law)

Energy (E) = V x I x t
```

## Specifications

| Feature | Specification |
|---------|---------------|
|Static Current Measurement Range| 1 nA to 200 mA|
|Dynamic Range| 100 nA to 50 mA (100 kHz bandwidth)|
|Sampling Rate| 3.2 Msamples/s |
|Power Measurement Range | 180 nW to 165 mW |
|Supply Voltage Output|1.8 V to 3.3 V (programmable)|
|Internal MCU|STM32L496VGT6 @ 80 MHz|
|Internal ADC|Three 12-bit ADCs at 5 Msamples/s|

## Measurement Modes

<b>Static Mode</b> Current averaged over time and results are displayed on the onboard LCD, no PC required. Useful for quick baseline checks of idle current consumption. Measurement range up to 200 mA.

<b>Dynamic Mode</b> This captures the current waveform as it changes during algorithm execution, so every microamp change is recorded. Real-time current measurement with 100 kHz bandwidth and up to 50 mA range.


## References

1. [STMicroelectronics (2021) UM2243 — User Manual: X-NUCLEO-LPM01A STM32 Power Shield](/docs/02%20-%20Power%20Measurement%20Shield/References/um2243-stm32-power-shield-xnucleoLPM01A-stmicroelectronics.pdf)