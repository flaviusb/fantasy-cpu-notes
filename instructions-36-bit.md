# Encoding for 36 bit system

x = op
i = immediate
a, b, c = displacement address, using -64 to +64 with no 0, similar to 1's complement - 0000000 is 1, which goes to 0111111 for 64, 1111111 is -64, 1000000 is -1
A, B, C = absolute address, word aligned
. = ?????
- = purely visual styling to make sections easier to see

type 2 000000-00xxxxxxxaaaaaaabbbbbbbccccccc
type 3 000000-010xxxxxxxxxxxxxaaaaaaabbbbbbb
type 4 000000-0110xxxxxxxiiiiiaaaaaaabbbbbbb
type 5 000000-0111xxxxxxxxxiiiiiiiiiiaaaaaaa
type 6 000000-10xxxxxiiiAAAAAAAAAABBBBBBBBBB
type 7 000000-11xxxxxxxxAAAAAAAAAABBBBBBBBBB
type 8 xxxxxx-AAAAAAAAAABBBBBBBBBBCCCCCCCCCC for op does not equal 000000

type 7
move
swap
accumulate
decumulate
not
jz
jnz
popcnt
clz
ctz
neg

type 8
add
sub
mul
div
rem
and
or
nor
nand
andn
orn
xnor
jeq
jne
fmac
max
