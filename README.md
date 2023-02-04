# stm32-spinner
The aim of this project is the development of an stm32-based arcade spinner to be used, for example, in Arkanoid gameplay.
Spinner acceleration table can be customized by editing the map in compute_steps2() function.

## Bill of materials
* [EA-JM01 EG STARTS Arcade Spinner](https://www.amazon.it/dp/B08CZFJ7DM?psc=1&ref=ppx_yo2ov_dt_b_product_details)
* [nucleo board](https://www.amazon.it/Dev-Board-NUCLEO-32-NUCLEO-F042K6-STMICROELECTRONICS/dp/B07R4BPTQ5/)
* USB cable

## Setup
First off, set all Spinner dip-switches to ON position. This will enable UART data transfer with 1ms period. The protocol is simple: when spinning clockwise the string  `0xff 0x00 0x00 0x01` is  emitted and the blue LED flashes. When spinning counterclockwise, the string `0xff 0x00 0x00 0xFE` is emitted and the red LED flashes. The UART settings are usual 115200 8N1.
Looking the the female connector with the nothces upwards, its pinout is: `Vin-GND-TX-RX`. Spinner Vin must be 5V. 3V3 won't work. Spinner RX signal is not used.
There is also the possibility to use simple pulses instead of UART protocol by connecting the the K+ and K- signals.
See relevant logic analyzer dumps in the `logs` directory for more details.   
Snooping a USB mouse USB HID traffic with Wireshark, it can be noted that HID reports are emitted every 8ms. Tests showed that reports emitted at a faster pace are ignored by the USB host. Therefore, a 8ms timer triggers the USB reporting. The contents of the report depend on the number of pulses received by the spinner during the 8ms slot in a non-linear way so to enable acceleration effect. See also dumps coming from a real Dell mouse in the logs directory.


## Pinout
|Signal           | STM32 IO |  Nucleo connector  |
|-----------------|:--------:|:------------------:|
| USB DP (Green)  |   PA12   |    CN3-5  [D2]     |
| USB DM (White)  |   PA11   |    CN3-13 [D10]    |
| Spinner UART    |   PA10   |    CN3-2  [D0]     |
| Fire Button     |   PA0    |    CN4-12 [A0]     |
| Onboard LED     |   PB3    |    CN4-15 [D13]    |
|    Debug1       |   PB4    |    CN3-15 [D12]    |
|    Debug2       |   PB5    |    CN3-14 [D11]    |
|    +5V          |          |    CN4-4           |
|    GND          |          |    CN3-4           |
|    GND          |          |    CN4-2           |



## TODO
* <s> Use interrupt-driven UART acquisition </s>
* <s> Add spinner acceleration support </s>
* <s> Add precise timer-based USB report triggering instead of SysTick </s> 
* <s> Add documentation on hardware setup </s>
* <s> Add Wireshark and Saleae logs </s>
* <s> Add support for button </s>

