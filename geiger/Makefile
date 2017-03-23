# This is a prototype Makefile. Modify it according to your needs.
# You should at least check the settings for
# DEVICE ....... The AVR device you compile for
# CLOCK ........ Target AVR clock rate in Hertz
# OBJECTS ...... The object files created from your source files. This list is
#                usually the same as the list of source files with suffix ".o".
# PROGRAMMER ... Options to avrdude which define the hardware you use for
#                uploading to the AVR and the interface where this hardware
#                is connected.
# FUSES ........ Parameters for avrdude to flash the fuses appropriately.

DEVICE     = attiny2313
CLOCK      = 8000000
PROGRAMMER = -c usbtiny
OBJECTS    = geiger.o
FUSES      = -U lfuse:w:0xee:m -U hfuse:w:0xdd:m -U efuse:w:0xff:m
AVRDUDE    = avrdude $(PROGRAMMER) -p $(DEVICE) -B 10 -F
COMPILE    = avr-gcc -Wall -Os -DF_CPU=$(CLOCK) -mmcu=$(DEVICE)

# symbolic targets:
all:	geiger.hex	size

main.o:

.c.o:
	$(COMPILE) -c $< -o $@

.S.o:
	$(COMPILE) -x assembler-with-cpp -c $< -o $@

.c.s:
	$(COMPILE) -S $< -o $@

flash:	geiger.hex  size
	$(AVRDUDE) -U flash:w:geiger.hex:i

fuse:
	$(AVRDUDE) $(FUSES)

size:	geiger.elf
	avr-size --mcu=$(DEVICE) --format=avr geiger.elf

ident:
	$(AVRDUDE)

clean:
	rm -f $(OBJECTS)
	rm -f geiger.hex geiger.elf geiger.map

# file targets:
geiger.elf: $(OBJECTS)
	$(COMPILE) -o geiger.elf -Wl,-Map,geiger.map $(OBJECTS)

geiger.hex: geiger.elf
	rm -f geiger.hex
	avr-objcopy -j .text -j .data -O ihex geiger.elf geiger.hex
