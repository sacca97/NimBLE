Usage
=====
This usage guide is for the nRF52840-DK board, which has a PCA10056 BSP.

Prerequisites
-------------

* Internet connection
* newt tool installed
* gcc, gdb, `jlink`_

.. _jlink: https://www.segger.com/downloads/jlink/

Board setup
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



Hello world (Blinky app)
----------------------------
The Blinky app is a simple application that blinks the LED on the board.

      .. code-block:: bash

         newt target create nrf52_blinky
         newt target set nrf52_blinky app=apps/blinky
         newt target set nrf52_blinky bsp=@apache-mynewt-core/hw/bsp/nordic_pca10056
         newt target set nrf52_blinky build_profile=debug
         newt target set nrf52_blinky cflags="-Wno-error"
         newt build nrf52_blinky
         newt run nrf52_blinky 0

BLE peripheral
----------------
The BLE peripheral app is a simple application that advertises itself as a BLE peripheral.

      .. code-block:: bash

         newt pkg new apps/ble_peripheral -t app
         newt target create nrf52_ble_peripheral
         newt target set nrf52_ble_peripheral app=apps/ble_peripheral
         newt target set nrf52_ble_peripheral bsp=@apache-mynewt-core/hw/bsp/nordic_pca10056
         newt target set nrf52_ble_peripheral build_profile=debug
         newt target set nrf52_ble_peripheral cflags="-Wno-error"
         newt build nrf52_ble_peripheral
         newt run nrf52_ble_peripheral 0

BLE central
----------------
The BLE central app is a simple application that scans for BLE peripherals.

      .. code-block:: bash

         newt pkg new apps/ble_central -t app
         newt target create nrf52_ble_central
         newt target set nrf52_ble_central app=apps/ble_central
         newt target set nrf52_ble_central bsp=@apache-mynewt-core/hw/bsp/nordic_pca10056
         newt target set nrf52_ble_central build_profile=debug
         newt target set nrf52_ble_central cflags="-Wno-error"
         newt build nrf52_ble_central
         newt run nrf52_ble_central 0

Board reset
-----------

In case you need to reset the board to its original conditions, you can use the erase command from JLinkExe.

.. code-block:: bash

   JLinkExe -device nRF52 -speed 4000 -if SWD

   J-Link> erase

.. note::

   After erasing the board, the bootloader will be gone. You will need to flash it again.