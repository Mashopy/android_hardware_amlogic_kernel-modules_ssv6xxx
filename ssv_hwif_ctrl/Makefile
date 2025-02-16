PLATFORMS = ssv6051

KBUILD_TOP := $(srctree)/$(src)
include $(KBUILD_TOP)/$(PLATFORMS).cfg
include $(KBUILD_TOP)/platform-config.mak

PWD := $(shell pwd)

ifeq ($(KERNELRELEASE),)
# current directory is driver
CFGDIR = $(PWD)/../../../../config

-include $(CFGDIR)/build_config.cfg
-include $(CFGDIR)/rules.make

endif
EXTRA_CFLAGS := -I$(KBUILD_TOP) -I$(KBUILD_TOP)/include
EXTRA_CFLAGS += -Wno-pointer-sign
KMODULE_NAME=ssv_hwif_ctrl


KBUILD_EXTRA_SYMBOLS += $(M)/../ssv6051/Module.symvers
KBUILD_EXTRA_SYMBOLS += $(M)/../ssv6x5x/Module.symvers

KERN_SRCS := hwif_ctrl/sdio/sdio.c

#KERN_SRCS += platforms/$(PLATFORMS)-generic-wlan.c
$(KMODULE_NAME)-y += $(KERN_SRCS_S:.S=.o)
$(KMODULE_NAME)-y += $(KERN_SRCS:.c=.o)

obj-m+= $(KMODULE_NAME).o

all:module strip

module:
	make  -C $(SSV_KERNEL_PATH) ARCH=$(ARCH) CROSS_COMPILE=$(CROSS_COMPILE) \
	EXTRA_CFLAGS="$(EXTRA_CFLAGS)" M=$(PWD) modules

install:
	install -p -m 644 $(KMODULE_NAME).ko $(KMODDESTDIR)

uninstall:
	rm -f $(KMODDESTDIR)/$(KMODULE_NAME).ko
    
strip:
	$(CROSS_COMPILE)strip --strip-unneeded $(KMODULE_NAME).ko

clean:
	make -C $(SSV_KERNEL_PATH) ARCH=$(ARCH) CROSS_COMPILE=$(CROSS_COMPILE) \
	M=$(PWD) clean

