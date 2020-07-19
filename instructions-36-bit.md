# Encoding for 36 bit system

x = op
i = immediate
a, b, c = displacement address, using -64 to +64 with no 0, similar to 1's complement - 0000000 is 1, which goes to 0111111 for 64, 1111111 is -64, 1000000 is -1
A, B, C = absolute address, word aligned
. = ?????
- = purely visual styling to make sections easier to see

000000-00xxxxxxxaaaaaaabbbbbbbccccccc
000000-01xxxxxxx.......aaaaaaabbbbbbb
000000-10...
000000-11xxxxxxxxAAAAAAAAAABBBBBBBBBB
xxxxxx-AAAAAAAAAABBBBBBBBBBCCCCCCCCCC for op does not equal 000000
