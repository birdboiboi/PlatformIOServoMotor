# =========================================================================
#   Unity - A Test Framework for C
#   ThrowTheSwitch.org
#   Copyright (c) 2007-24 Mike Karlesky, Mark VanderVoord, & Greg Williams
#   SPDX-License-Identifier: MIT
# =========================================================================

#We try to detect the OS we are running on, and adjust commands as needed
ifeq ($(OS),Windows_NT)
  ifeq ($(shell uname -s),) # not in a bash-like shell
	CLEANUP = del /F /Q
	MKDIR = mkdir
  else # in a bash-like shell, like msys
	CLEANUP = rm -f
	MKDIR = mkdir -p
  endif
	TARGET_EXTENSION=.exe
else
	CLEANUP = rm -f
	MKDIR = mkdir -p
	TARGET_EXTENSION=.out
endif

C_COMPILER=g++
ifeq ($(shell uname -s), Darwin)
C_COMPILER=clang++
endif
UNITY_ROOT=../unity_src/

CFLAGS=-std=c89
CFLAGS += -Wall
CFLAGS += -Wextra
CFLAGS += -Wpointer-arith
CFLAGS += -Wcast-align
CFLAGS += -Wwrite-strings
CFLAGS += -Wswitch-default
CFLAGS += -Wunreachable-code
CFLAGS += -Winit-self
CFLAGS += -Wmissing-field-initializers
CFLAGS += -Wno-unknown-pragmas
CFLAGS += -Wstrict-prototypes
CFLAGS += -Wundef
CFLAGS += -Wold-style-definition
#CFLAGS += -Wno-misleading-indentation

TARGET_BASE1=test1
TARGET1 = $(TARGET_BASE1)$(TARGET_EXTENSION)
SRC_FILES1=$(UNITY_ROOT)/src/unity.c test/test_control.cpp src/productionCode/controller.cpp
INC_DIRS=-Isrc -I$(UNITY_ROOT)/src -Isrc -I src/productionCode
SYMBOLS=

all: clean default

default: $(SRC_FILES1)
	$(C_COMPILER) $(CFLAGS) $(INC_DIRS) $(SYMBOLS) $(SRC_FILES1) -o $(TARGET1)
	- ./$(TARGET1)

test/test_runners/TestProductionCode_Runner.c: test/TestProductionCode.c
	ruby $(UNITY_ROOT)/auto/generate_test_runner.rb test/TestProductionCode.c  test/test_runners/TestProductionCode_Runner.c
	ruby $(UNITY_ROOT)/auto/generate_test_runner.rb test/TestProductionCode2.c test/test_runners/TestProductionCode2_Runner.c

clean:
	$(CLEANUP) $(TARGET1) $(TARGET2)

ci: CFLAGS += -Werror
ci: default
