QCA6564 driver for Digi Embedded Yocto (DEY)
=============================================
The QCA6564 driver is a Linux kernel driver for the Qualcomm
QCA6564 wireless chipset.

The Linux driver is maintained upstream at [coreaurora](https://source.codeaurora.org/quic/la/platform/vendor/qcom-opensource/wlan/qcacld-2.0/log/?h=caf-wlan/QCA6564_LE_1.0.3_LA.4.2.2.3).

This repository is a fork which maintains patches for Digi Embedded product
range.

Firmware / Configuration Files
------------------------------
This driver requires the QCA6564 firmware, which is avlaible in a spereate repository.

It requires the firmware_bin/WCNSS_cfg.dat and firmware_bin/WCNSS_qcom_cfg.ini configuration files from this repository
to be copied into /lib/firmware/wlan on the target system, and renamed cfg.dat, qcom_cfg.ini.

It also requires a wlan_mac.bin file on the target, in the /lib/firmware/wlan directory, the format is:
Intf0MacAddress=<MAC>, ie. Intf0MacAddress=0004F3FFFFFB

Compiling the Driver
--------------------

Example of compiling with buildroot, showing the required configuration settings to build for the Digi imx6ul SOM:

```
################################################################################                                                              
#                                                                                                                                             
# driver-qca6564                                                                                                                              
#                                                                                                                                             
################################################################################                                                              
                                                                                                                                              
DRIVER_QCA6564_VERSION = 97d8df0b97e236b21b3bd83fca63a2dc90e85cee                                                                             
DRIVER_QCA6564_SITE = $(call github,ccxtechnologies,driver-qca6564,$(DRIVER_QCA6564_VERSION))                                                 
DRIVER_QCA6564_LICENSE = ISC, BSD-like, GPL-2.0+                                                                                              
                                                                                                                                              
DRIVER_QCA6564_MODULE_MAKE_OPTS = WLAN_ROOT=$(@D)                                                                                             
DRIVER_QCA6564_MODULE_MAKE_OPTS += MODNAME=wlan                                                                                               
DRIVER_QCA6564_MODULE_MAKE_OPTS += CONFIG_QCA_WIFI_ISOC=0                                                                                     
DRIVER_QCA6564_MODULE_MAKE_OPTS += CONFIG_QCA_WIFI_2_0=1                                                                                      
DRIVER_QCA6564_MODULE_MAKE_OPTS += CONFIG_QCA_CLD_WLAN=m                                                                                      
DRIVER_QCA6564_MODULE_MAKE_OPTS += WLAN_OPEN_SOURCE=1                                                                                         
DRIVER_QCA6564_MODULE_MAKE_OPTS += CONFIG_CLD_HL_SDIO_CORE=y                                                                                  
DRIVER_QCA6564_MODULE_MAKE_OPTS += CONFIG_LINUX_QCMBR=y                                                                                       
DRIVER_QCA6564_MODULE_MAKE_OPTS += WLAN_OPEN_SOURCE=1                                                                                         
DRIVER_QCA6564_MODULE_MAKE_OPTS += CONFIG_NON_QC_PLATFORM=y                                                                                   
DRIVER_QCA6564_MODULE_MAKE_OPTS += BUILD_DEBUG_VERSION=0                                                                                      
                                                                                                                                              
define DRIVER_QCA6564_INSTALL_MODPROBE                                                                                                        
    mkdir -p $(TARGET_DIR)/etc/modprobe.d                                                                                                     
    $(INSTALL) -D -m 644 $(@D)/modprobe-qualcomm.conf $(TARGET_DIR)/etc/modprobe.d/qualcomm.conf                                              
                                                                                                                                              
    mkdir -p $(TARGET_DIR)/lib/firmware/wlan                                                                                                  
    install -m 0644 $(@D)/firmware_bin/WCNSS_cfg.dat $(TARGET_DIR)/lib/firmware/wlan/cfg.dat                                                  
    install -m 0644 $(@D)/firmware_bin/WCNSS_qcom_cfg.ini $(TARGET_DIR)/lib/firmware/wlan/qcom_cfg.ini                                        
                                                                                                                                              
endef                                                                                                                                         
DRIVER_QCA6564_POST_INSTALL_TARGET_HOOKS += DRIVER_QCA6564_INSTALL_MODPROBE                                                                   
                                                                                                                                              
$(eval $(kernel-module))                                                                                                                      
$(eval $(generic-package))  
```

License
-------
Copyright 2017, Digi International Inc.

Copyright (c) 2012-2015 The Linux Foundation. All rights reserved.

Previously licensed under the ISC license by Qualcomm Atheros, Inc.

Permission to use, copy, modify, and/or distribute this software for
any purpose with or without fee is hereby granted, provided that the
above copyright notice and this permission notice appear in all
copies.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL
WARRANTIES WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED
WARRANTIES OF MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE
AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL
DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR
PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER
TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR
PERFORMANCE OF THIS SOFTWARE.
