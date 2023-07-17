.. _usage-guide-basics:

Usage
=====
This usage guide is for the nRF52840-DK board, which has a PCA10056 BSP.

Prerequisites
-------------

* Internet connection
* newt tool installed
* gcc, gdb, `jlink`_

.. _jlink: https://www.segger.com/downloads/jlink/

Project setup
----------------------

#. Initialize a new project

      .. code-block:: bash

         newt new --shallow=0 project_name

         cd project_name

         newt upgrade


#. Build and flash the bootloader
   
      .. code-block:: bash

         newt target create nrf52_boot
         newt target set nrf52_boot app=@mcuboot/boot/mynewt
         newt target set nrf52_boot bsp=@apache-mynewt-core/hw/bsp/nordic_pca10056
         newt target set nrf52_boot build_profile=optimized
         newt target set nrf52_boot cflags="-Wno-error"
         newt build nrf52_boot
         newt load nrf52_boot


#. Build and run a blinky app

      .. code-block:: bash

         newt target create nrf52_blinky
         newt target set nrf52_blinky app=apps/blinky
         newt target set nrf52_blinky bsp=@apache-mynewt-core/hw/bsp/nordic_pca10056
         newt target set nrf52_blinky build_profile=debug
         newt target set nrf52_blinky cflags="-Wno-error"
         newt build nrf52_blinky
         newt run nrf52_blinky 0

Board reset
-----------

In case you need to reset the board to its original conditions, you can use the erase command from JLinkExe.

.. code-block:: bash

   JLinkExe -device nRF52 -speed 4000 -if SWD

   J-Link> erase

