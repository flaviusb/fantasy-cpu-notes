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


'Devices' are a thing.

Ringbuffers attached to a clock + a mux + a DAC, populate the ringbuffer to make sound happen.

Emulate storage with a file/set of files that the emulator connects a virtual device to.

Have some forms of media be nybble addressable, so that file streams work between 36 and 64 bit systems, and to make conversion to/from legacy systems easier.

Have some forms of memory where you can send an operation - one of the 16 binary ops, plus an operand, to perform MEM = MEM OP OPERAND on the memory itself. You can do in place single bit set/clear with OR/ORN, for instance.

MMIO - three ways.
- Mount data plane, translucent control plane
  - Eg mount a range of remote memory, writes on either side will turn into bus traffic to 'write through'
    - Control plane not 'fully transparent' as mounting/unmounting as well as things like stride/window are explicit through the arbitrator
- Mount control plane, translucent data plane
  - Eg set memory addresses to correspond to control registers, reads and writes get tagged and sent through to the device which hangs off the end of the memory bus
    - Data is translucent in that often you will have to manually frame/unframe the data in through the control registers
- Mount control plane and data plane, with explicit write through policy for data plane set by control plane
  - Eg this lets you blit chunks of memory back and forth with stall state for when it has not completed the fetch.

Also needed for consideration: hierarchical submounts, eg for a window manager splitting up screen memory across different loci.
