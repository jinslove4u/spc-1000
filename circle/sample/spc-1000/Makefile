#
# Makefile
#
#

#ifeq ($(strip $(DEPTH)), 8)
#LIBCIRCLE = libcircle8
#else
#LIBCIRCLE = libcircle
#endif

CIRCLEHOME = ../..
INCLUDE = -I$(CIRCLEHOME)/addon

#OBJS	= main.o kernel.o
OBJS = main.o kernel.o Z80SPC.o spcall.o MC6847.o AY8910.o pwmsound.o casswindow.o

LIBS =$(CIRCLEHOME)/addon/ugui/libugui.a \
	  $(CIRCLEHOME)/lib/usb/libusb.a \
	  $(CIRCLEHOME)/lib/input/libinput.a \
	  $(CIRCLEHOME)/lib/fs/fat/libfatfs.a \
	  $(CIRCLEHOME)/lib/fs/libfs.a \
	  $(CIRCLEHOME)/lib/$(LIBCIRCLE).a 

EXTRACLEAN = sound.h

BMP = samsung

include ../Rules.mk

sound.h: sound.h.gz
	gunzip -c sound.h.gz > sound.h

%.bin: %.asm
	z80asm -fb -l -sa $< 
spcall.c: spc-1000_all.bin spc-1000_ipl.bin
	cat spc-1000_all.bin spc-1000_ipl.bin > spcall.rom
	bin2c.py spcall.rom ROM

bootcode: bootcode.bin start.elf fixup.dat
	wget https://github.com/raspberrypi/firmware/blob/master/boot/$<?raw=true -O $<	
