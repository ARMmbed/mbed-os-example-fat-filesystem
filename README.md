# Obsoleted

**This repository have been superceded by following application:**

* [mbed OS example Filesystem](https://github.com/ARMmbed/mbed-os-example-filesystem)

# Getting started with the FAT file system on Mbed OS

This guide reviews the steps to get the FAT file system working on an Mbed OS platform.

Please install [Mbed CLI](https://os.mbed.com/docs/latest/tools/setup.html).

## Hardware requirements

This example uses a RAM-backed FAT file system. The FAT file system requires at least 128 512-byte blocks for a total of 64KB of space. The HeapBlockDevice, which uses a target's heap for storage, backs this space. Therefore, to support this example, a target must have at least 64KB of space usable by as heap. Because there is more than just the heap in a device's RAM, this translates to the requirement that a target's RAM must be at least 96KB large.

## Import the example application

From the command-line, import the example:

```
mbed import mbed-os-example-fat-filesystem
cd mbed-os-example-fat-filesystem
```

### Now compile

Invoke `mbed compile`, and specify the name of your platform and your favorite toolchain (`GCC_ARM`, `ARM`, `IAR`). For example, for the ARM Compiler 5:

```
mbed compile -m K64F -t ARM
```

Your PC may take a few minutes to compile your code. At the end, you see the following result:

```
[snip]
+--------------------------+-------+-------+-------+
| Module                   | .text | .data |  .bss |
+--------------------------+-------+-------+-------+
| Fill                     |   164 |     0 |  2136 |
| Misc                     | 54505 |  2556 |   754 |
| drivers                  |   640 |     0 |    32 |
| features/filesystem      | 15793 |     0 |   550 |
| features/storage         |    42 |     0 |   184 |
| hal                      |   418 |     0 |     8 |
| platform                 |  2355 |    20 |   582 |
| rtos                     |   135 |     4 |     4 |
| rtos/rtx                 |  5861 |    20 |  6870 |
| targets/TARGET_Freescale |  8382 |    12 |   384 |
| Subtotals                | 88295 |  2612 | 11504 |
+--------------------------+-------+-------+-------+
Allocated Heap: 24576 bytes
Allocated Stack: unknown
Total Static RAM memory (data + bss): 14116 bytes
Total RAM memory (data + bss + heap + stack): 38692 bytes
Total Flash memory (text + data + misc): 91947 bytes

Image: ./BUILD/K64F/gcc_arm/mbed-os-example-fat-filesystem.bin
```

### Program your board

1. Connect your Mbed device to the computer over USB.
1. Copy the binary file to the Mbed device.
1. Press the reset button to start the program.
1. Open the UART of the board in your favorite UART viewing program. For example, `screen /dev/ttyACM0`.

**Note:** The default serial port baud rate is 9600 bit/s.

You see the following output:

```
Welcome to the filesystem example.
Formatting a FAT, RAM-backed filesystem. done.
Mounting the filesystem on "/fs". done.
Opening a new file, numbers.txt. done.
Writing decimal numbers to a file (20/20) done.
Closing file. done.
Re-opening file read-only. done.
Dumping file to screen.
0
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
EOF.
Closing file. done.
Opening root directory. done.
Printing all filenames:
  numbers.txt
Closeing root directory. done.
Filesystem Demo complete.

```

## Switch from RAM backed block device to an SD card

From the command-line, run the following command:

```bash
mbed add sd-driver
```

Then change the code on line 3 of `main.cpp` to import the SD card header:

```C
#include "SDBlockDevice.h"
```

Change the block device declaration on line 7 of `main.cpp` to use the SD card by replacing the `PinName`s with the pins connected to the SD card:

```C
SDBlockDevice bd(PinName mosi, PinName miso, PinName sclk, PinName cs);
```

## Troubleshooting

If you have problems, you can review the [documentation](https://os.mbed.com/docs/latest/tutorials/debugging.html) for suggestions on what could be wrong and how to fix it.
