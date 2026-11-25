.. _snippet-riscv-smode-aia-imsic:

RISC-V S-mode AIA with IMSIC Snippet (riscv-smode-aia-imsic)
############################################################

.. code-block:: console

   west build -S riscv-smode-aia-imsic [...]

Overview
********

This snippet makes a RISC-V board that runs Zephyr in S-mode use the Advanced
Interrupt Architecture (AIA) instead of the PLIC. The wired interrupts go
through the supervisor-level APLIC domain in MSI delivery mode and are delivered
by the supervisor-level IMSIC interrupt file of each hart.

It is supported on the ``opensbi`` and ``opensbi_smp`` variants of
:zephyr:board:`qemu_riscv64`, where it also makes QEMU emulate the AIA:

.. code-block:: console

   west build -b qemu_riscv64/qemu_virt_riscv64/opensbi -S riscv-smode-aia-imsic samples/hello_world
   west build -t run
