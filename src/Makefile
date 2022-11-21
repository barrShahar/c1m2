#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# <Put a Description Here>
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <Put a description of the supported targets here>
#
# Platform Overrides:
#      <Put a description of the supported Overrides here
#
#------------------------------------------------------------------------------
include sources.mk

# Paths

INC =

# Platform Overrides
PLATFORM = HOME

# Architectures Specific Flags
LINKER_FILE = ${LINKER_PATH}msp432p401r.lds
CPU = 
ARCH = 
SPECS = 


# Compiler Flags and Defines
CC = gcc
LD = 
LDFLAGS = 
GFLAGS = -Wall -Werror -g -O0 -std=c99  # For both platforms (general flags)
CFLAGS = 
MSPFLAGS = -mcpu=$(CPU) -m${ARCH} -march=armv7e-m \
           -mfloat-abi=hard -mfpu=fpv4-sp-d16 --specs=$(SPECS)
CPPFLAGs = 

# Additional variables variables
TARGET = c1m2
SRCS =
debug = 

I_FILES = $(SOURCES:.c=.i)

ifeq ($(PLATFORM),MSP432)
	CC = arm-none-eabi-gcc
	CFLAGS = ${GFLAGS} ${MSPFLAGS} -DMSP432
	SRCS = ${SOURCES_MSP}
	LDFLAGS = -Wl,-Map=$(TARGET).map -T $(LINKER_FILE)
	CPU = cortex-m4
	ARCH = thumb
	SPECS = nosys.specs
	INC = ${INC_COMMON} ${INC_MSP} ${INC_CMSIS}


# etc
else
	CC = gcc
	CFLAGS = ${GFLAGS} -DHOST
	SRCS = ${SOURCES_HOME}
	INC = ${INC_COMMON}
	LDFLAGS = -Xlinker -Map=${TARGET}.map
# etc
endif

OBJS = $(SRCS:.c=.o)
OBJDEB = $(SRCS:.c=.d)

# ifeq (${PLATFORM}, HOME):
# 	debug = cat memory.c
# endif


.PHONY debug:
debug:
	${CC} ${INC}  ${SRCS} ${CFLAGS} ${LDFLAGS}

.PHONY build:
build : ${TARGET}.out ${TARGET}.map


${TARGET}.out : ${OBJS} ${OBJDEB}
	${CC} ${INC} ${CFLAGS} ${OBJS} -o $@ ${LDFLAGS}

.PHONY compile-all:
compile-all: ${OBJS}


%.o : %.c 
	${CC} ${INC} ${CFLAGS} -c $< 
%.i : %.c 
	${CC} ${INC} ${CFLAGS} -E $< -o $@

%.asm : %.c # do you need INC_COMMON? 
	${CC} ${INC} ${CFLAGS} -S $< -o $@

%.d : %.c 
	${CC} ${INC} ${CFLAGS} -M $< -o $@

.PHONY clean:
clean:
	rm -f *.o *.i *.asm *.d ${TARGET}.out ${TARGET}.map