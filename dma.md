DMA controllers accept command streams, and then do the memory transfers between (or within) connected memory banks.

2 (not mutually exclusive) possibilities for how to 'wait until completion before attempting use':

1. DMA into a region causes stalls on access while the DMA operates
2. Interrupts on completion

For (2), the interrupt number and CPu number to pass it to could be given to the DMA controller as a parameter within the command stream.

If we have threads as such, where the hardware is aware of their abi, interrupts could be linked into thread sleep/wake.

---

Parameters:
- Device/Memory bank to/from
- Buffer, if any
- starting byte, length
- strides + widths
- horizontal/vertical braiding
- priority
