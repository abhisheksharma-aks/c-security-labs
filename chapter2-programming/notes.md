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
0x55555555514f <main+15>:       0xc7    0x45    0xf8    0x00    0x00    0x00    0x00    0x83
(gdb) x/8xw $rip
0x55555555514f <main+15>:       0x00f845c7      0x83000000      0x7d0af87d      0x3d8d4819
0x55555555515f <main+31>:       0x00000ea1      0xc6e800b0      0x8bfffffe      0xc083f845

Here, the last address at the first position, with the bytes reversed. This same byte
reversal effect can be seen when a full four-byte word is shown. 
This is because on the x86 processor values are store in little-endian byte order, which 
means the least significant byte is stored first. 


(gdb) i r $rip
rip            0x55555555514f      0x55555555514f <main+15>
(gdb) x/4xb $rip
0x55555555514f <main+15>:	0xc7	0x45	0xf8	0x00
(gdb) x/4ub $rip
0x55555555514f <main+15>:	199	69	248	0
(gdb) x/luw $rip
0x55555555514f <main+15>:	16270791
(gdb) x/lxw $rip
0x55555555514f <main+15>:	0x00f845c7
(gdb) quit
A debugging session is active.

	Inferior 1 [process 5092] will be killed.

Quit anyway? (y or n) y

~/Documents/c-security-labs/chapter2-programming main* 1m 8s
❯ bc -ql
199 + (69 * 256) + (248 * 256^2) + (0 * 256^3)
16270791
quit

Here, we can see in the calculator too, that when we use the byte order of x/4ub $rip 
on the calculator and use the least significant bit at the higher power, we get the same
address as x/luw $rip. 
