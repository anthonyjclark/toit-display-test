# OLED Display Code with Error

Steps taken:

1. Install `jaguar` with `brew install toitlang/toit/jag`
1. Check jaguar version with `jag version`

  ~~~bash
  Version:         v1.39.0
  SDK version:     v2.0.0-alpha.159
  Build date:      2024-08-15T19:39:54Z
  ~~~

1. Connect device
1. Run `jag flash --chip esp32s3-spiram-octo`

  ~~~bash
  Flashing device over serial on port '/dev/cu.usbmodem14201' ...
  esptool.py v4.6
  Serial port /dev/cu.usbmodem14201
  Connecting...
  Chip is ESP32-S3 (revision v0.2)
  Features: WiFi, BLE
  Crystal is 40MHz
  MAC: 64:e8:33:51:3a:dc
  Uploading stub...
  Running stub...
  Stub running...
  Changing baud rate to 921600
  Changed.
  Configuring flash size...
  Flash will be erased from 0x00000000 to 0x00002fff...
  Flash will be erased from 0x00008000 to 0x00008fff...
  Flash will be erased from 0x0000d000 to 0x0000efff...
  Flash will be erased from 0x00010000 to 0x0016afff...
  Compressed 12112 bytes to 8831...
  Wrote 12112 bytes (8831 compressed) at 0x00000000 in 0.2 seconds (effective 478.1 kbit/s)...
  Hash of data verified.
  Compressed 4096 bytes to 171...
  Wrote 4096 bytes (171 compressed) at 0x00008000 in 0.1 seconds (effective 628.8 kbit/s)...
  Hash of data verified.
  Compressed 8192 bytes to 31...
  Wrote 8192 bytes (31 compressed) at 0x0000d000 in 0.1 seconds (effective 777.5 kbit/s)...
  Hash of data verified.
  Compressed 1421200 bytes to 914352...
  Wrote 1421200 bytes (914352 compressed) at 0x00010000 in 9.9 seconds (effective 1144.5 kbit/s)...
  Hash of data verified.

  Leaving...
  Hard resetting via RTS pin...
  ~~~

1. Run `jag monitor`

  ~~~bash
  Starting serial monitor of port '/dev/cu.usbmodem14201' ...
  [toit] INFO: starting <v2.0.0-alpha.159>
  ESP-ROM:esp32s3-20210327
  Build:Mar 27 2021
  rst:0x15 (USB_UART_CHIP_RESET),boot:0x2a (SPI_FAST_FLASH_BOOT)
  Saved PC:0x403846e6
  SPIWP:0xee
  mode:DIO, clock div:1
  load:0x3fce3810,len:0x88
  load:0x403c9700,len:0x96c
  load:0x403cc700,len:0x2504
  entry 0x403c9850
  [toit] INFO: starting <v2.0.0-alpha.159>
  [toit] DEBUG: clearing RTC memory: powered on by hardware source
  [toit] INFO: running on ESP32S3 - revision 0.2
  [toit] INFO: using SPIRAM for heap metadata and heap
  [wifi] DEBUG: connecting
  [wifi] DEBUG: connected
  [wifi] INFO: network address dynamically assigned through dhcp {ip: 172.28.81.122}
  [wifi] INFO: dns server address dynamically assigned through dhcp {ip: [208.67.220.220, 208.67.222.222]}
  [jaguar.http] INFO: running Jaguar device 'friendly-net' (id: '7c5bb79e-a636-4880-b189-50230ffb35a3') on 'http://172.28.81.122:9000'
  ~~~

1. Create new package with

  ~~~bash
  mkdir Display
  cd Display
  jag pkg init
  jag pkg install github.com/toitware/toit-ssd1306@v2
  jag pkg install toitware/toit-pixel-display@v2
  ~~~

1. Create file `display.toit`
1. Run with `jag run display.toit --device 172.28.81.122`

  ~~~bash
  [jaguar] INFO: program c6925192-2d45-548f-7f78-726e3c457cad started
  Heap report @ out of memory in primitive 6:2:
    ┌───────────┬──────────┬─────────────────────────────────────────────────────┐
    │   Bytes   │  Count   │  Type                                               │
    ├───────────┼──────────┼─────────────────────────────────────────────────────┤
    │   40960   │     12   │  toit processes                                     │
    │     16384 │        4 │    system    0 be455840-e70a-2712-836f-c73308e7e172 │
    │     20480 │        5 │    other     1 77a590d1-4fd3-b113-b784-1b2234ecf97a │
    │      4096 │        1 │    current   2 c6925192-2d45-548f-7f78-726e3c457cad │
    │  667648   │      1   │  heap metadata                                      │
    │    4096   │      1   │  spare new-space                                    │
    │   16808   │      7   │  heap overhead                                      │
    │     192   │      4   │  thread/other                                       │
    └───────────┴──────────┴─────────────────────────────────────────────────────┘
    Total: 729704 bytes in 16 allocations (8%), largest free 7468k, total free 7475k

  ******************************************************************************
  Decoding by `jag`, device has version <2.0.0-alpha.159>
  ******************************************************************************
  MALLOC_FAILED error.
    0: Bus.test                  <sdk>/i2c.toit:65:5
    1: Bus.scan                  <sdk>/i2c.toit:60:10
    2: main                      display.toit:23:18
  ******************************************************************************

  [jaguar] ERROR: program c6925192-2d45-548f-7f78-726e3c457cad stopped - exit code 1
  ~~~

1. Try with different envelope: `jag flash --chip esp32s3` (same flashing output more or less)
1. Run `jag monitor`

  ~~~bash
  Starting serial monitor of port '/dev/cu.usbmodem14201' ...
  E (327) quad_psram: PSRAM ID read error: 0x00ffffff, PSRAM chip ESP-ROM:esp32s3-20210327
  Build:Mar 27 2021
  rst:0x15 (USB_UART_CHIP_RESET),boot:0x2a (SPI_FAST_FLASH_BOOT)
  Saved PC:0x403846a2
  SPIWP:0xee
  mode:DIO, clock div:1
  load:0x3fce3810,len:0x88
  load:0x403c9700,len:0x96c
  load:0x403cc700,len:0x2504
  entry 0x403c9850
  E (299) quad_psram: PSRAM ID read error: 0x00ffffff, PSRAM chip not found or not supported, or wrong PSRAM line mode
  E (300) esp_psram: PSRAM enabled but initialization failed. Bailing out.
  [toit] INFO: starting <v2.0.0-alpha.159>
  [toit] DEBUG: clearing RTC memory: powered on by hardware source
  [toit] INFO: running on ESP32S3 - revision 0.2
  [wifi] DEBUG: connecting
  [wifi] DEBUG: connected
  [wifi] INFO: network address dynamically assigned through dhcp {ip: 172.28.81.122}
  [wifi] INFO: dns server address dynamically assigned through dhcp {ip: [208.67.220.220, 208.67.222.222]}
  [jaguar.http] INFO: running Jaguar device 'calm-trip' (id: '109f3496-c29a-4f9a-ad5f-8b9e8a1a30e3') on 'http://172.28.81.122:9000'
  ~~~

1. (notice the error messages above)
1. Run with `jag run display.toit --device 172.28.81.122`
1. This works.

## Thoughts

1. There is a runtime error message when using `esp32s3-spiram-octo` that is not present when using `esp32s3`.
2. I cannot use `jag run ...` unless I have an additional terminal open with `jag monitor` running.
3. My network does not allow to automatic discovery of devices with `jag scan`.
4. I'm worried that I won't have enough memory for everything I need to do if I cannot rely on PSRAM.
