# 编译命令 : make

SHELL = /bin/bash

# 引入一些环境参数
-include ./mk_def

# 从目录检索需要编译的c文件
MY_DIRS := ./lua-5.3.5/src ./lua-cjson-2.1.0 ./luafilesystem-2.0/src
MY_DIRS += ./lpeg-1.0.2

# 编译选项
MY_CFLAGS := $(my_flags)
MY_CFLAGS += -std=gnu99 -D_LIB

# 引用头文件
MY_INCLUDES := -I ./lua-5.3.5/src
MY_INCLUDES += -I ./inc

# 引用的静态库
MY_LIB_STATIC := -L $(my_lib_path) -Bstatic

# 引用的动态库
MY_LIB_DYNAMIC := -L $(my_lib_path) -Bdynamic -lm -ldl

# 编译目标名称
MY_TARGET_NAME := lua-5.3.5
MY_TARGET := $(my_lib_path)/lib$(MY_TARGET_NAME)
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


.PHONY: all clean

all: lib

lib: $(MY_TARGET_A)
so: $(MY_TARGET_SO)

%.c.o: %.c
	$(CC) $(MY_A_PARAMS) $(MY_LIB_MINI) -c -o $@ $<

%.c.oo: %.c
	$(CC) $(MY_SO_PARAMS) -c -o $@ $<

$(MY_TARGET_A): $(MY_LIB_A_OBJS)
	$(my_tip)
	$(CAR) rs $(MY_TARGET_A) $(MY_LIB_A_OBJS)
	$(CRANLIB) $(MY_TARGET_A)
	
$(MY_TARGET_SO): $(MY_LIB_SO_OBJS)
	$(my_tip)
	$(CC) -shared -fPIC -o $@ $(MY_LIB_SO_OBJS) $(MY_SO_PARAMS) $(CXX_LINK_MINI)


prefix:


install:
#	$(CP) 


clean:
	@echo "++++++ make clean ++++++"
	@echo "+ MY_DIRS = $(MY_DIRS)"
	@echo "++ RM = $(RM)"
	$(RM) $(MY_LIB_A_OBJS)
	$(RM) $(MY_LIB_SO_OBJS)
	$(RM) $(MY_TARGET)
	$(RM) $(MY_TARGET_A)
	$(RM) $(MY_TARGET_SO)
	@echo "+++++++++++++++++++++++++"

define my_tip
	@echo "++++++ make tip ++++++"
	@echo "+ CC = $(CC)"
	@echo "+ CXX = $(CXX)"
	@echo "+ MY_SOURCES = $(MY_SOURCES)"
	@echo "+ MY_DIRS = $(MY_DIRS)"
	@echo "+ MY_CFLAGS = $(MY_CFLAGS)"
	@echo "+ MY_TARGET = $(MY_TARGET)"
	@echo "+ MY_TARGET_A = $(MY_TARGET_A)"
	@echo "+ MY_TARGET_SO = $(MY_TARGET_SO)"
	@echo "++++++++++++++++++++++"
endef
