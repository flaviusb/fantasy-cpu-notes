Architectures to crib heavily off: DEC Alpha, MMIX.
Architectures to crib a little off, maybe: POWER8, Sparc.

Most novel feature: Memory architecture.
Very non-uniform memory. No cache as such at all.
L1 is the only thing that the IP can point into. All 'cache' control
is essentially manual. Lots of DMA stuff, with fairly complex memory shuffling commands.
All load, store, move etc instructions deal with L1 only. No cache coherence!
L1 is per core.

Relatedly:
- Lots of DMA controllers.
- Various devices controlled via pushing to 'command buffers'.
- Devices are addressed by id.
- All mappings (ids, pointers etc) may vary between cores under certain circumstances, though they will remap 'logically'.
- That is, eg device id #0 will alway be the local most DMA controller.

General features:
- Fixed encoding (probably 32 bits per instruction)
- Most instructions take either 2 or 3 registers - (source -> dest) or (source, source -> dest)
- Some instructions have a variant where one of the source registers is replaced by an inline 8 bit value
- 256 registers, with register 0 being hard-wired to be zero
- Condition register/s for conditional jump etc
