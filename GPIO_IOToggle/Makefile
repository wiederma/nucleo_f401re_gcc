# put your *.o targets here, make should handle the rest!

SRCS = main.c stm32f4xx_it.c system_stm32f4xx.c

# all the files will be generated with this name (main.elf, main.bin, main.hex, etc)

PROJ_NAME=main

# that's it, no need to change anything below this line!

###################################################

CC=arm-none-eabi-gcc
OBJCOPY=arm-none-eabi-objcopy

CFLAGS  = -g -O2 -Wall -Tstm32_flash.ld 
CFLAGS += -mlittle-endian -mcpu=cortex-m4 -mthumb -mthumb-interwork
CFLAGS += -mfloat-abi=hard -mfpu=fpv4-sp-d16

###################################################

vpath %.c src
vpath %.a lib

ROOT=$(shell pwd)

CFLAGS += -Iinc 
CFLAGS += -Ilib/CMSIS/inc
CFLAGS += -Ilib/CMSIS/Device/STM32F4xx-Nucleo/inc
#CFLAGS += -Ilib/CMSIS/Device/STM32F4xx/inc

CFLAGS += -Ilib/Drivers/STM32F4xx-Nucleo/

CFLAGS += -Ilib/STM32F4xx_HAL_Driver/inc
#CFLAGS += -Ilib/StdPeriph_Driver/inc

SRCS += lib/CMSIS/Device/STM32F4xx-Nucleo/startup/startup_stm32f401xe.s # add startup file to build
#SRCS += lib/CMSIS/Device/STM32F4xx-Nucleo/startup/startup_stm32f4xx.s # add startup file to build

OBJS = $(SRCS:.c=.o)

###################################################

.PHONY: lib proj

all: lib proj

lib:
	$(MAKE) -C lib/STM32F4xx_HAL_Driver
#	$(MAKE) -C lib/StdPeriph_Driver

proj: 	$(PROJ_NAME).elf

$(PROJ_NAME).elf: $(SRCS)
	$(CC) $(CFLAGS) $^ -o $@ -Llib/STM32F4xx_HAL_Driver/ -lstm32f4
#	$(CC) $(CFLAGS) $^ -o $@ -Llib/StdPeriph_Driver/ -lstm32f4
	$(OBJCOPY) -O ihex $(PROJ_NAME).elf $(PROJ_NAME).hex
	$(OBJCOPY) -O binary $(PROJ_NAME).elf $(PROJ_NAME).bin

clean:
	rm -f *.o
	rm -f lib/StdPeriph_Driver/*.o lib/StdPeriph_Driver/libstm32f4.a
	rm -f lib/STM32F4xx_HAL_Driver/*.o lib/STM32F4xx_HAL_Driver/libstm32f4.a
	rm -f $(PROJ_NAME).elf
	rm -f $(PROJ_NAME).hex
	rm -f $(PROJ_NAME).bin
