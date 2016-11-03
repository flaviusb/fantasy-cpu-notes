Architectures to crib heavily off for basic run cores: DEC Alpha, MMIX.
Architectures to crib a little off, maybe, for basic run cores: POWER8, Sparc.

Very non-uniform memory. No cache as such at all. Cores have access to various pools of memory, with different properties; these are per core, though some are also accesible to channel processors.
There is a special, very small run memory, which is the only thing that the IP can point into. All 'cache' control
is essentially manual. All load, store, move etc instructions deal with these pools accessible to the core only. No cache coherence!
Executables are made up of chunked run images, which get 'linked' by channel programs when they are moved into a run pool.


Relatedly:
- Lots of channel processors.
- Hardware, mixed and software processes.
- NoC
- Various devices controlled via messages sent over the NoC, in various ways.
- Both the NoC and the processors are non uniform - there are subnets with different properties, and there are cores with different properties.
- Recursive nameservers are a thing.

General features of the generic 'software running cores':
- Fixed encoding (probably 32 bits per instruction)
- Most instructions take either 2 or 3 registers - (source -> dest) or (source, source -> dest)
- Some instructions have a variant where one of the source registers is replaced by an inline 8 bit value
- 256 registers, with register 0 being hard-wired to be zero
- Condition register/s for conditional jump etc
