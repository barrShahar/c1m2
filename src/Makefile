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
INC_COMMON = ${INCLUDES}/common/ # ../include/common/ ??
INC_MSP    = ${INCLUDES}/msp432/
INC_CMSIS  = ${INCLUDES}/CMSIS/
INC =

# Platform Overrides
PLATFORM = HOME

# Architectures Specific Flags
LINKER_FILE = msp432p401r.lds
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
TARGET = c1m2.out
SRCS =
debug = 

I_FILES = $(SOURCES:.c=.i)

ifeq ($(PLATFORM),MSP432)
	CC = gcc
	CFLAGS = ${GFLAGS} ${MSPFLAGS} -DMSP432
	SRCS = ${SOURCES_MSP}
	LDFLAGS = -Wl,-Map=$(TARGET).map -T $(LINKER_FILE)
	CPU = cortex-m4
	ARCH = thumb
	SPECS = nosys.specs
	INC = -I${INC_COMMON} -I${INC_MSP}
# etc
else
	CC = gcc
	CFLAGS = ${GFLAGS} -DHOST
	SRCS = ${SOURCES_HOME}
	INC = ${INC_COMMON}
	LDFLAGS = 
# etc
endif

OBJS = $(SRCS:.c=.o)
# ifeq (${PLATFORM}, HOME):
# 	debug = cat memory.c
# endif


.PHONY debug:
debug:
	echo ${LDFLAGS}

.PHONY build:
build : ${TARGET}

.PHONY Build:
Build: ${TARGET}

${TARGET} : ${OBJS}
	gcc ${LDFLAGS} ${OBJS}  -o $@ 

.PHONY compile-all:
compile-all: ${OBJS}


%.o : %.c 
	${CC} -I${INC_COMMON} ${CFLAGS} -c $< 
%.i : %.c 
	${CC} -I${INC_COMMON} ${CFLAGS} -E $< -o $@

%.asm : %.c # do you need INC_COMMON? 
	${CC} ${INC} ${CFLAGS} -S $< -o $@

.PHONY clean:
clean:
	rm -f *.o *.i *.asm ${TARGET}.out ${TARGET}.map