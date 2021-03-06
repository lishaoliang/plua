﻿# 编译命令 : make

SHELL = /bin/bash

# 引入一些环境参数
-include ../src/mk_def

# 从目录检索需要编译的c文件
MY_DIRS := ../src/lua-5.3.5/app


# 编译选项
MY_CFLAGS := $(my_flags)
MY_CFLAGS += -std=gnu99 -DBGFX_STATIC_LINK

# 引用头文件
MY_INCLUDES := -I ../src/lua-5.3.5/src
MY_INCLUDES += -I ../src/inc -I ../../klb/inc -I ../../klb/test
MY_INCLUDES += -I ../bgfx/bx/include -I ../bgfx/bimg/include -I ../bgfx/bgfx/include -I ../bgfx/lua-bgfx

# 引用的静态库
MY_LIB_STATIC := -L $(my_lib_path) -Bstatic -lklb_test -lklb -llua-5.3.5
MY_LIB_STATIC += -L ../bgfx/bgfx/.build/linux64_gcc/bin -Bstatic -lbgfxRelease -lbimg_encodeRelease -lbimgRelease -lbxRelease -lGLU -lGL -lX11

# 引用的动态库
MY_LIB_DYNAMIC := -L $(my_lib_path) -Bdynamic -lm -ldl -lpthread

# 编译目标名称
MY_TARGET_NAME := klua
MY_TARGET_EXE := $(my_lib_path)/$(MY_TARGET_NAME)
MY_TARGET_A := $(my_lib_path)/lib$(MY_TARGET_NAME).a
MY_TARGET_SO := $(my_lib_path)/lib$(MY_TARGET_NAME).so


# 编译器
CC := $(my_cc)
CXX := $(my_cxx)
CAR := $(my_ar)
CRANLIB := $(my_ranlib)
CSTRIP := $(my_strip)
CPREFIX := $(my_prefix)
MAKE := make
RM := -rm -f
CP := cp


# 所有编译文件
MY_FIND_FILES_C = $(wildcard $(dir)/*.c)
MY_SOURCES = $(foreach dir, $(MY_DIRS), $(MY_FIND_FILES_C))


MY_LIB_A_OBJS := $(addsuffix .o, $(MY_SOURCES))
MY_A_PARAMS := $(MY_INCLUDES) $(MY_CFLAGS) $(MY_LIB_STATIC) $(MY_LIB_DYNAMIC)

MY_LIB_SO_OBJS := $(addsuffix .oo, $(MY_SOURCES))
MY_SO_PARAMS := -fPIC $(MY_INCLUDES) $(MY_CFLAGS) $(MY_LIB_STATIC) $(MY_LIB_DYNAMIC) 


# 编译静态库时候,使compiler为每个function和data item分配独立的section
MY_LIB_MINI = -ffunction-sections -fdata-sections

# 编译动态库或执行档时,使compiler删除所有未被使用的function和data,即编译之后的文件最小化
MY_LINK_MINI = -Wl,--gc-sections


.PHONY: all clean install

all: exe

exe: $(MY_TARGET_EXE)
lib: $(MY_TARGET_A)
so: $(MY_TARGET_SO)

%.c.o: %.c
	$(CC) $(MY_A_PARAMS) $(MY_LIB_MINI) -c -o $@ $<

%.c.oo: %.c
	$(CC) $(MY_SO_PARAMS) -c -o $@ $<

depends:
	if [ -d ../src ]; then $(MAKE) -C ../src; fi
	if [ -d ../../klb ]; then $(MAKE) -C ../../klb; fi
	if [ -d ../../klb/test ]; then $(MAKE) -C ../../klb/test; fi

$(MY_TARGET_EXE): depends $(MY_LIB_A_OBJS)
	$(my_tip)
	$(CXX) -o $@ $(MY_LIB_A_OBJS) $(MY_A_PARAMS) $(MY_LINK_MINI)

$(MY_TARGET_A): $(MY_LIB_A_OBJS)
	$(my_tip)
	$(CAR) rs $(MY_TARGET_A) $(MY_LIB_A_OBJS)
	$(CRANLIB) $(MY_TARGET_A)
	
$(MY_TARGET_SO): $(MY_LIB_SO_OBJS)
	$(my_tip)
	$(CC) -shared -fPIC -o $@ $(MY_LIB_SO_OBJS) $(MY_SO_PARAMS) $(CXX_LINK_MINI)

clean:
	if [ -d ../src ]; then $(MAKE) -C ../src clean; fi
	if [ -d ../../klb ]; then $(MAKE) -C ../../klb clean; fi
	if [ -d ../../klb/test ]; then $(MAKE) -C ../../klb/test clean; fi

	@echo "++++++ make clean ++++++"
	@echo "+ MY_DIRS = $(MY_DIRS)"
	@echo "++ RM = $(RM)"
	$(RM) $(MY_LIB_A_OBJS)
	$(RM) $(MY_LIB_SO_OBJS)
	$(RM) $(MY_TARGET_EXE)
	$(RM) $(MY_TARGET_A)
	$(RM) $(MY_TARGET_SO)
	@echo "+++++++++++++++++++++++++"

strip:
	$(CSTRIP) $(MY_TARGET_EXE)

install:
	$(CP) $(MY_TARGET_EXE) $(INSTALL_BIN)/

define my_tip
	@echo "++++++ make tip ++++++"
	@echo "+ CC = $(CC)"
	@echo "+ CXX = $(CXX)"
	@echo "+ MY_SOURCES = $(MY_SOURCES)"
	@echo "+ MY_DIRS = $(MY_DIRS)"
	@echo "+ MY_CFLAGS = $(MY_CFLAGS)"
	@echo "+ MY_TARGET_EXE = $(MY_TARGET_EXE)"
	@echo "+ MY_TARGET_A = $(MY_TARGET_A)"
	@echo "+ MY_TARGET_SO = $(MY_TARGET_SO)"
	@echo "++++++++++++++++++++++"
endef
