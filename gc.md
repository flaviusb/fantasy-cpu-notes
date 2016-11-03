GC happens either close to memory, or close to arbitration. Most GC is done in hardware.

There are several kinds of GC units:
- Small state machines, generated parameterised on a single type of data structure, that manage a typed pool, and use up one of the pools ports to do scanning and freeing every cycle.
- Modules that act as part of an actual memory arbitration unit, possibly several stages deep.
- A paging unit.
- A facet of a channel chip, which deals with complex patterns of allocation and freeing.
