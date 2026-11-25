.. zephyr:board:: k3_com260

Overview
********

The SpacemiT K3 CoM260 is a computer-on-module with the SpacemiT K3, a 64-bit
RISC-V SoC with eight X100 and eight A100 cores that implements the RISC-V
Advanced Interrupt Architecture (AIA).

Zephyr runs in S-mode as a payload of the OpenSBI firmware of the board, on the
first X100 core by default. It uses the supervisor-level APLIC and IMSIC as
interrupt controllers and the SBI timer.

With :kconfig:option:`CONFIG_SMP` enabled Zephyr uses all sixteen cores, which
it starts through the SBI HSM extension. Set
:kconfig:option:`CONFIG_MP_MAX_NUM_CPUS` to 8 to keep it on the X100 cores.

Supported Features
==================

.. zephyr:board-supported-hw::

Programming and Debugging
*************************

.. zephyr:board-supported-runners::

Building
========

Applications for the ``k3_com260`` board can be built as usual (see
:ref:`build_an_application`):

.. zephyr-app-commands::
   :zephyr-app: samples/hello_world
   :board: k3_com260
   :goals: build

Booting
=======

The image is linked to run from ``0x140000000``. The U-Boot of the board only
boots FIT images, so wrap :file:`zephyr.bin` in one with that address as load and
entry address:

.. code-block:: devicetree

   /dts-v1/;

   / {
       #address-cells = <2>;

       images {
           zephyr {
               data = /incbin/("zephyr.bin");
               type = "kernel";
               arch = "riscv";
               os = "linux";
               compression = "none";
               load = <0x1 0x40000000>;
               entry = <0x1 0x40000000>;
           };
       };

       configurations {
           default = "conf";
           conf {
               kernel = "zephyr";
           };
       };
   };

.. code-block:: console

   mkimage -f zephyr.its zephyr.itb

Then load it from U-Boot, for instance from the first partition of an SD card,
and boot it:

.. code-block:: console

   fatload mmc 0:1 0x150000000 zephyr.itb
   bootm 0x150000000 - ${fdtcontroladdr}

The console is on UART0 at 115200 8N1.
