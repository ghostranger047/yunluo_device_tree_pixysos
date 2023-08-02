###############################################################################
# Necessary Check

DRIVER_ROOT := $(KERNEL_SRC)/../mediatek-modules/connectivity/wlan/adaptor

ifneq ($(KERNEL_OUT),)
    ccflags-y += -imacros $(KERNEL_OUT)/include/generated/autoconf.h
endif

ifndef TOP
    TOP := $(srctree)/..
endif

# Force build fail on modpost warning
KBUILD_MODPOST_FAIL_ON_WARNINGS := y

ccflags-y += \
    -I$(srctree)/drivers/misc/mediatek/include/mt-plat \
    -I$(KERNEL_SRC)/../mediatek-modules/connectivity/common/common_main/include \
    -I$(KERNEL_SRC)/../mediatek-modules/connectivity/common/common_main/linux/include

ifeq ($(CONNAC_VER), 2_0)
ccflags-y += -I$(KERNEL_SRC)/../mediatek-modules/connectivity/conninfra/include
ccflags-y += -I$(KERNEL_SRC)/../mediatek-modules/connectivity/conninfra/debug_utility
ccflags-y += -I$(KERNEL_SRC)/../mediatek-modules/connectivity/conninfra/debug_utility/include
ccflags-y += -I$(KERNEL_SRC)/../mediatek-modules/connectivity/conninfra/debug_utility/connsyslog
ccflags-y += -I$(KERNEL_SRC)/../mediatek-modules/connectivity/conninfra/debug_utility/coredump
endif

ifneq ($(CONFIG_MTK_CONNSYS_DEDICATED_LOG_PATH),)
ccflags-y += -DCONFIG_MTK_CONNSYS_DEDICATED_LOG_PATH
ccflags-y += -I$(KERNEL_SRC)/../mediatek-modules/connectivity/common/debug_utility
ccflags-y += -I$(DRIVER_ROOT)/include
endif

ifeq ($(CONFIG_MTK_CONN_LTE_IDC_SUPPORT),y)
    ccflags-y += -DWMT_IDC_SUPPORT=1
else
    ccflags-y += -DWMT_IDC_SUPPORT=0
endif

ifeq ($(CONNAC_VER), 2_0)
ccflags-y += -DCFG_ANDORID_CONNINFRA_SUPPORT=1
ccflags-y += -DCFG_ANDORID_CONNINFRA_COREDUMP_SUPPORT=1
else
ccflags-y += -DCFG_ANDORID_CONNINFRA_SUPPORT=0
ccflags-y += -DCFG_ANDORID_CONNINFRA_COREDUMP_SUPPORT=0
endif

ccflags-y += -D MTK_WCN_WMT_STP_EXP_SYMBOL_ABSTRACT

ccflags-y += -D CREATE_NODE_DYNAMIC=1

ifeq ($(CONFIG_WLAN_DRV_BUILD_IN),y)
obj-y += $(ADAPTOR_MODULE_NAME).o
else
obj-m += $(ADAPTOR_MODULE_NAME).o
endif

# Wi-Fi character device driver
$(ADAPTOR_MODULE_NAME)-objs += wmt_cdev_wifi.o
ifneq ($(CONFIG_MTK_CONNSYS_DEDICATED_LOG_PATH),)
$(ADAPTOR_MODULE_NAME)-objs += fw_log_wifi.o
$(ADAPTOR_MODULE_NAME)-objs += fw_log_ics.o
$(ADAPTOR_MODULE_NAME)-objs += wlan_ring.o
endif
ifeq ($(CONNAC_VER), 2_0)
$(ADAPTOR_MODULE_NAME)-objs += wifi_pwr_on.o
endif

extra_symbols := $(abspath $(O)/../mediatek-modules)/connectivity/common/Module.symvers

all:
	$(MAKE) -C $(KERNEL_SRC) M=$(M) modules $(KBUILD_OPTIONS) KBUILD_EXTRA_SYMBOLS=$(extra_symbols)

modules_install:
	$(MAKE) M=$(M) -C $(KERNEL_SRC) modules_install

clean:
	$(MAKE) -C $(KERNEL_SRC) M=$(M) clean
