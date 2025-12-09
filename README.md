# HML087 emulator using the microprocessor ch32v003 and the library ch32fun

The ROM HML087 is used in the BMW E30 coding plug (codierstecker) connected to the Service Interval (SI) board.

This software emulates the HML087 using a microcontroller. 

The microcontroller WCH CH32V003J4M6 was selected because it supports +5 Volt power, has 8 pins (SOP-8), and its speed works with the latest SI boards.  
It can be programmed using the WCH-LinkE programmer. Connect +5V (or 3.3V) from the programmer to the coder pin 3, GND to the pin 8, SWIO to the pin 4. 

If you want to test CH32V003, you can get a development board for a version of the chip with more pins.  

Chips, development boards, and programmers are widely available on Aliexpress. The chip costs 10 cents.

The communication protocol between the SI board and coding plug is simple: when the CS pin is set high by the microcontroller on the SI board,
the SI's microcontroller sends 64 clock signals on the CLOCK pin. On each clock signal change from high to low,
the DATA pin of the coder outputs data: 0 or 1.

The data is stored in the array 'coder' for different coding plugs in the files like 1385468.h (1385468 is written on the front of the coder plug for the M20).

Download the library ch32fun from https://github.com/cnlohr/ch32fun
and set the path to it in Makefile.

Also, select one of the coders in Makefile.

Compile by running:
make

Use a data analyzer to get data for a coder that is not listed. Also, a 2-channel oscilloscope can be used. You need to examine only 64 bits in one batch, 
there are usually 3 subsequent batches.
   
The SI board reads the coder only when it starts. The coder's microcontroller polls the GPIO inputs in a circle without delays to return data as fast as possible.  
After a few minutes of CS inactivity, the code puts the microcontroller into the deep sleep state. The microcontroller can be awaken by the CS signal.

The GPIO pins of the microcontroller for the signals CS, CLOCK, and DATA were selected to simplify routing without vias.

A more detailed description of the schema and board is here: https://hobby.farit.ru/hml087-coder-rom-for-e30-instrument-cluster/

