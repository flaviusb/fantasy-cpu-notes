We make extensive use of channel processors instead of DMA, MMUs, or IOMMUs.

Channel processors accept command streams in their own various machine languages. They can do several things:
- Memory transfers within and between connected memory banks, with or without data transformation.
- Negotiation memory arbitration and allocation.
- Writing to or reading from peripherals, busses, GPIOs, ports, or buffers of various kinds, depending on how they are connected, and setting timers and interrupts for same.
- Certain kinds of NoC traffic.
- GC and cleanup of resources.
- Swizzling and program transformation. This is needed as the final linking stage happens whenever executable image segments are transferred into
  the run memory of a core.
- Fetching for a core, or pushing from one core to another.
- Sending wake and sleep messages for threads/processes/nodes/cores.


Basic parameters for the basic send instruction:
- Device/Memory bank to/from
- Buffer, if any
- starting byte, length
- strides + widths
- horizontal/vertical braiding
- priority
