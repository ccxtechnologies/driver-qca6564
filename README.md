QCA6564 driver for Digi Embedded Yocto (DEY)
=============================================
The QCA6564 driver is a Linux kernel driver for the Qualcomm
QCA6564 wireless chipset.

The Linux driver is maintained upstream at [coreaurora](https://source.codeaurora.org/quic/la/platform/vendor/qcom-opensource/wlan/qcacld-2.0/log/?h=caf-wlan/QCA6564_LE_1.0.3_LA.4.2.2.3).

This repository is a fork which maintains patches for Digi Embedded product
range.

Compiling the driver
--------------------
The QCA6564 is meant to be used by Digi Embedded Yocto, which includes
recipes to build the package. It's also part of the default images generated
by Digi Embedded Yocto.

It can be also compiled using a Yocto based toolchain. In that case, make
sure to source the corresponding toolchain of the platform you want to build
the connector for, e.g:

```
. <DEY-toolchain-path>/environment-setup-cortexa7hf-vfp-neon-dey-linux-gnueabi
QCA_EXTRA_FLAGS="CONFIG_CLD_HL_SDIO_CORE=y CONFIG_LINUX_QCMBR=y WLAN_OPEN_SOURCE=1 CONFIG_NON_QC_PLATFORM=y"
make -j4 ${QCA_EXTRA_FLAGS} M=${QCA_SRC} KLIB_BUILD=${KERNEL_SRC} KERNEL_PATH=${KERNEL_SRC} KERNEL_SRC=${KERNEL_SRC}
```

Where ${KERNEL_SRC} is the patch to the configured and compiled Linux kernel
source the module will be loaded into.

More information about [Digi Embedded Yocto](https://github.com/digi-embedded/meta-digi).

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
