Here is the command to set GDB permanently to intel syntax.
Through gdb - 
	gdb -q
		set disassembly-flavor intel
Through terminal - 
	echo "set disassembly-flavor intel" >> ~/.gdbinit

The assembly instructions in Intel syntax generally follow this style:

	operation <destination>, <source>

The destination and source values will either be a register, a memory address or a value.
mov - move a value from source to destination 
sub - subract a value
inc - increment a value
cmp - compare values
	Any operation beginning with j is used to jump to a different part of the code 
	(depending on the result of the comparison)
lea - load effective address

We can use examine command in GDB to look at certain address fo memory in a variety of 
ways. This command expects two arguments when it's used: the in location in memory to 
examine and how to display that memory.

The display format also uses a single-letter shorthand, which is optionally preceded by 
a count of how many items to examine. Some common format letters are as follows:

o - display in octal
x - display in hexadecimal
u - display in ungsigned, standard base-10 decimal
t - display in binary

These can be used with the examine command to examine a certain memory address. 
Example - 
info register rip / i r rip (shorthand)
rip   		0x8048348		0x8048348<main+16>
(gdb) x/o $rip / 0x8048348
0x8048348<main+16>: 		077042707

The memory the RIP register is pointing to can be determined by using the address stored
in the RIP. The debugger lets you reference registers directly, so $rip is equivalent to 
the value RIP contains at that moment. 

The default sie of single unit is a four-byte unit called a word. This size of the display
units for the examine command can be changed by adding a size letter to the end of the 
format letter. the valid size letter are as follows:

b - a single byte
h - a halfword, which is two bytes in size
w - a word, which is four bytes in size
g - a giant, which is eight bytes in size

Remember, the word used here is different from the terminology that intel WORD uses, 
in intel terminology WORD is 2 bytes, DWORD is 4 bytes and QWORD is 8 bytes in any 
architecture. 

Examples -
(gdb) x/8xb $rip
0x55555555514f <main+15>:	0xc7	0x45	0xf8	0x00	0x00	0x00	0x00	0x83
(gdb) x/8xw $rip
0x55555555514f <main+15>:	0x00f845c7	0x83000000	0x7d0af87d	0x3d8d4819
0x55555555515f <main+31>:	0x00000ea1	0xc6e800b0	0x8bfffffe	0xc083f845
(gdb) x/8xh $rip
0x55555555514f <main+15>:	0x45c7	0x00f8	0x0000	0x8300	0xf87d	0x7d0a	0x4819	0x3d8d


