.. zephyr:board:: devebox_stm32h7xx_m

Overview
********

The DevEBox STM32H7XX M is a compact core board (DevEBox / mcudev.taobao.com)
built around an STM32H743VIT6, marketed under the misprinted silkscreen
"STM32F7XX_M" (note the F, should be H). It closely resembles the WeAct
MiniSTM32H743 board but differs in LED/button pins, has no card-detect line
on the microSD socket, and breaks the LCD header out over SPI2 instead of
SPI4.

Key Features

- STM32H743VIT6 in LQFP100 package
- USB Micro-B OTG full-speed device
- 1 user LED (D2, PA1), 1 power LED (D1, fixed on 3.3V rail)
- 2 user push-buttons (K1/PE3, K2/PC5) + reset button
- 25 MHz HSE and 32.768 kHz LSE crystal oscillators
- 64-Mbit (8 MiB) external QSPI NOR flash (W25Q64JV)
- microSD card slot (no card-detect pin)
- 8-pin LCD/OLED header (SPI2-based, no fixed panel)
- DCMI camera FPC connector
- SWD header
- 2x 22-pin GPIO expansion headers

Default Zephyr Peripheral Mapping
==================================

- USER_LED : PA1 (sink-driven, active low)
- K1 / K2 : PE3 / PC5 (active low, internal pull-up)
- SPI2 SCK/MISO/MOSI/NSS : PB13/PB14/PB15/PB12 (LCD header)
- LCD D/C : PB1, LCD Backlight : PB0
- QuadSPI CLK/NCS/IO0/IO1/IO2/IO3 : PB2/PB6/PD11/PD12/PE2/PD13 (NOR flash)
- SDMMC1 CLK/CMD/D0/D1/D2/D3 : PC12/PD2/PC8/PC9/PC10/PC11 (microSD, no CD)
- USB DM/DP : PA11/PA12
- I2C2 SCL/SDA : PB10/PB11 (DCMI camera connector)

System Clock
============

Driven by the main PLL at 240 MHz, fed by the 25 MHz HSE crystal (Y2).

Programming and Debugging
**************************

BOOT0 is tied to GND through R7 (10K) by default, so the board boots from
main flash. To enter the USB DFU bootloader, tie BOOT0 to 3V3 (e.g. via the
SWD header's BT0 pin) and reset.

.. zephyr-app-commands::
   :zephyr-app: samples/basic/blinky
   :board: devebox_stm32h7xx_m
   :goals: build flash
   :gen-args: -DCONFIG_BOOT_DELAY=5000

An SWD header (J1) is also present for use with an external debugger
(e.g. ST-Link) via the jlink or openocd runners.
