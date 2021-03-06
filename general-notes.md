Architectures to crib heavily off for basic run cores: SuperH2 (and the other sh's), DEC Alpha.
Architectures to crib a little off, maybe, for basic run cores: POWER8, Sparc, MMIX, Boneless, Octavo, ReRISC.

Very non-uniform memory. No cache as such at all. Cores have access to various pools of memory, with different properties; these are per core, though some are also accesible to channel processors.
There is a special, very small run memory, which is the only thing that the IP can point into. All 'cache' control
is essentially manual. All load, store, move etc instructions deal with these pools accessible to the core only. No cache coherence!
Executables are made up of chunked run images, which get 'linked' by channel programs when they are moved into a run pool.

Possibilities:
36-bit word, 10-bit addressable scratch, architecture for synthesis into FPGAs
- Several instruction formats:
  * leading 000000 splits into
    - following 0xxxxxxxx, 3 7-bit displacements (-64 to +64 with no 0, similar to 1's complement - 0000000 is 1, which goes to 0111111 for 64, 1111111 is -64, 1000000 is -1)
    - following 1xxxxxxxxx is 9 bits operand, 2 10 bit addresses, with subformats where the operand is shorter and there is an immediate embedded
  * Anything else is 6 bits operand (except 000000), 3 10 bit addresses
64-bit word, 19-bit addressable scratch, architecture for ASIC
- Two instruction formats:
  * leading 0000000 is 19 bits operand, 2 19 bit addresses, with subformats where the operand is shorter and there is an immediate embedded
  * Anything else is 7 bits op (except 0000000), 3 19-bit operands
- Immediate variations, some fused ops (eg x + a shift)

Options (summary)
- 36 bit word
 * 36864 bits of scratch, 36 bit alignment, for 1024 words, 10-bit addressable
 * up to 3 address instructions, addressing into scratch
- 48-bit word
 * 786432 bits of scratch, 48-bit alignment, for 16384 words, 14-bit addressable
 * up to 3 address instructions, addressing into scratch
- 64-bit word
 * 33554432 bits of scratch, 64 bit alignment, for 524288 words, 19-bit addressable
 * up to 3 address instructions, addressing into scratch

Commonalities:
- Each word or scratch has a corresponding set of flags to determine
  * Mapping state for MMIOs
  * Stall state if waiting on a remote write
  * Dependency on channel processors
- A set of ops in common, and an extension set that differ
- In common: Integer/FP arithmetic packed/unpacked/signed/unsigned, Logical and/or/nor/xor/not/andn/orn/xnor, shift/rotate, 'bitops' like popcnt/clz/coalesce/etc, Map/Unmap local device, Send NoC message? (maybe do this through map/unmapping a NoC endpoint?), conditional/ternaries like cmov, maybe jz/jnz/jcmp? (Or maybe do this by logical operator with a dest register of 0x0?)

Relatedly:
- Lots of channel processors, arbitration processors, and a deep integration of resource handling between OS and hardware for eg memory, IO, processes, threads, mailboxes, supervisors, arbitrators, wake/sleep triggers, tokens etc.
- Hardware, mixed and software processes.
- NoC
- Various devices controlled via messages sent over the NoC, in various ways.
- Both the NoC and the processors are non uniform - there are subnets with different properties, and there are cores with different properties.
- Recursive nameservers are a thing.

Think in distsys terms fractally - implement backpressure not just in interconnect, but in resources as well (eg memory, processor time).

---

General features of the generic 'software running cores':
- No registers, no flags
- 13 bit addresses addressing into 4 byte aligned scratch for a total of 32kB addressable scratch
- 32 bit encoding, with the following formats
  - leading 1 bit literal '0', then 4 bits operand, 1 bit sink selector, 2 × 13 bits of address
  - leading 2 bits literal '10', then 9 bits of operand, 13 bits of address
  - leading 3 bits literal '110', then 6 bits of operand, 8 bits of constant, 2 bits of byte selection, 13 bits of address
  - leading 3 bits literal '111', then 29 bits of operand
- Some opcodes use direct addresses, some use indirect addresses. For those that use indirect addresses, they may use either one or two address values (loosely) packed into each 32 bit addressable unit; the first address starts at byte 0, and the second address (if any) starts at byte 2.

No registers and no flags, only scratch. The consequence of this is special addresses which the processor uses for these purposes. This is both an architectural point and also part of the ABI.
These special addresses can still be set to 'stall' state, from eg channel processor operations. If the processor accesses one of these while it is in the 'stall' state, it stalls, just as if it tried to read from or write to the address for any other reason.

Two kinds of libraries - inner and outer. Inner libraries are compiled into the program binary, and are used using the usual function calling ABI. There is no boundary between the library and the program; a misbehaving inner library can trash the program. Outer libraries run on the other side of a boundary, and can only be communicated with through IPC; a misbehaving outer library cannot directly affect the state of the program.
