---
title: "Zephyr Exploration"
date: 2024-08-09T20:15:46+05:30
draft: false
---

# Exploring Zephyr RTOS

Hi everyone! Back with another technical exploration! So, I recently stumbled upon the [BeagleConnect Freedom](https://www.beagleboard.org/boards/beagleconnect-freedom), and with it came the opportunity to explore [Zephyr RTOS](https://www.zephyrproject.org/). I have extensively worked with FreeRTOS and the ESP-IDF framework, and I was curious to see how Zephyr compares to it.

My aim is to drive the onboard ADC, control some I2C peripherals, and control some motors using PWM. My comments come from a perspective an intermediate ESP-IDF developer, so excited to see how Zephyr fares in comparison.

## Setting up Zephyr

Installation was a bit confusing. The [document](https://docs.beagleboard.org/latest/boards/beagleconnect/freedom/demos-and-tutorials/using-zephyr.html) present at BeagleBoard organization's website for installation of Zephyr was a mess! There were no instructions for my platform as well, I ended up referring the Zephyr documentation for installation of the SDK and the tools.

Here are the steps I followed:

1. First, install the sdk as mentioned on [Zephyr's getting started section](https://docs.zephyrproject.org/latest/develop/getting_started/index.html)

2. Once installed, I went ahead with setting up the BeagleConnect Freedom(BCF)'s SDK. (Referred from [this section](https://docs.beagleboard.org/latest/boards/beagleplay/demos-and-tutorials/zephyr-cc1352-development.html#beagleplay-zephyr-development-setup) of Beagleboard's documentation)

```bash
west init -m https://git.beagleboard.org/beagleconnect/zephyr/zephyr --mr sdk zephyr-beagle-cc1352-sdk
cd $HOME/zephyr-beagle-cc1352-sdk
python3 -m venv zephyr-beagle-cc1352-env
echo "export ZEPHYR_TOOLCHAIN_VARIANT=zephyr" >> $HOME/zephyr-beagle-cc1352-sdk/zephyr-beagle-cc1352-env/bin/activate
echo "export ZEPHYR_SDK_INSTALL_DIR=$HOME/zephyr-sdk-0.15.1" >> $HOME/zephyr-beagle-cc1352-sdk/zephyr-beagle-cc1352-env/bin/activate
echo "export ZEPHYR_BASE=$HOME/zephyr-beagle-cc1352-sdk/zephyr" >> $HOME/zephyr-beagle-cc1352-sdk/zephyr-beagle-cc1352-env/bin/activate
echo 'export PATH=$HOME/zephyr-beagle-cc1352-sdk/zephyr/scripts:$PATH' >> $HOME/zephyr-beagle-cc1352-sdk/zephyr-beagle-cc1352-env/bin/activate
echo "export BOARD=beagleconnect_freedom" >> $HOME/zephyr-beagle-cc1352-sdk/zephyr-beagle-cc1352-env/bin/activate
source $HOME/zephyr-beagle-cc1352-sdk/zephyr-beagle-cc1352-env/bin/activate
west update
west zephyr-export
pip3 install -r zephyr/scripts/requirements-base.txt
```

Beware, you might need to modify the exported variables as per your Zephyr installation done in section 1.

After this, getting an example program running was as simple as:

```bash
cd zephyr-beagle-cc1352-sdk
source zephyr-beagle-cc1352-env/bin/activate
west build -d build/play_blinky zephyr/samples/basic/blinky
west flash --build-dir build/play_blinky
```

## Exploring the code (and encountering a slight issue)

There seems to be some sample codes present inside the `zephyr/samples` directory. Let's have a look at the blinky example, which uses PWM as well `blinky_pwm`. 

```cpp
/*
 * Copyright (c) 2016 Intel Corporation
 * Copyright (c) 2020 Nordic Semiconductor ASA
 *
 * SPDX-License-Identifier: Apache-2.0
 */
```

Do companies contribute to zephyr themselves?? Why does Intel and Nordic own copyright to this file?!?! Also how does copyright play out for open source code?

```cpp
#include <zephyr/kernel.h>
#include <zephyr/sys/printk.h>
#include <zephyr/device.h>
#include <zephyr/drivers/pwm.h>
```

okay so someone has already written a driver for PWM nice. Won't it need PWM generators on the hardware? I guess information about the PWM generators will be pulled from the device tree. I will check out beagleconnect freedom's device tree to see if there are any PWM devices.

Looking at `cc13x2_cc26x2.dtsi`, `cc1352r7.dtsi` and `beagleconnect_freedom.dts`, I did not find a single PWM peripheral? How will PWM be generated on this MCU? 

On trying to build the `blinky_pwm` example for the board, I get this warning:

```cmake
CMake Warning at /Users/anon/zephyr-beagle-cc1352-sdk/zephyr/CMakeLists.txt:870 (message):
  No SOURCES given to Zephyr library: drivers__pwm
```

And this error:

```cmake
/Users/anon/zephyr-beagle-cc1352-sdk/zephyr/include/zephyr/device.h:85:41: error: '__device_dts_ord_DT_N_ALIAS_pwm_led0_P_pwms_IDX_0_PH_ORD' undeclared here (not in a function)
   85 | #define DEVICE_NAME_GET(dev_id) _CONCAT(__device_, dev_id)
```

Therefore, let's check out the ADC example. Again, this only compiles till this point:

```cmake
/Users/anon/zephyr-beagle-cc1352-sdk/zephyr/samples/drivers/adc/src/main.c:20:2: error: #error "No suitable devicetree overlay specified"
   20 | #error "No suitable devicetree overlay specified"
      |  ^~~~~
```

After asking on the forums, it turns out that the BCF ADC and PWM changes were recently merged, so I had to use the mainline zephyr source code. 

```bash
cd zephyr
git remote add upstream https://github.com/zephyrproject-rtos/zephyr
git pull upstream main
git branch --set-upstream-to=upstream/main
cd ..
west update
```

And now I could build the ADC and PWM files!

This post has gotten big and slightly off-topic. Mostly because I am rambling without focus. Let me continue the exploration in a next post. I guess since my last post was 6 months old, I must also post an update (sorry for ghosting folks!).

Toodles!

Ada