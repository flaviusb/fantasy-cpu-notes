An executable is a streaming, seekable container format for chunks of instructions and data for cores, plus instructions for how to demux, link, and route it.

Demuxing and linking instructions take the form of channel instructions.

Executable images are set up to stream in chunks. Within each chunk you have relative jumps only. Between chunks you have relocatable addresses, which are filled in by linking, which happens in the channel processor as it is streamed in.

There are several chunk sizes, but they are all pretty small - 256 bytes * a power of two.

Executables have to be compiled for a given computer layout - that is, a set of cores, with type, bram size etc information, and a given topology.

The chunks within the singular image can of course target different core types (eg cpu, gpu, channel, as well as variations), and this is specified in the demuxing and routing instructions.

I am not sure yet how to do libraries.
