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

The examine command also accepts the format letter i, short for instruction, to display 
the memory as disassembled assembly language instructions.


gdb -q ./a.out
Reading symbols from ./a.out...
(gdb) break main
Breakpoint 1 at 0x114f: file firstprog.c, line 5.
(gdb) run
Starting program: /home/mindcarrier/Documents/c-security-labs/chapter2-programming/a.out

This GDB supports auto-downloading debuginfo from the following URLs:
  <https://debuginfod.archlinux.org>
  <https://debuginfod.cachyos.org>
Enable debuginfod for this session? (y or [n]) y
Debuginfod has been enabled.
To make this setting permanent, add 'set debuginfod enabled on' to .gdbinit.
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/usr/lib/libthread_db.so.1".

Breakpoint 1, main () at firstprog.c:5
5		for(i=0; i < 10; i++)			//Loop 10 times
(gdb) i r $rip
rip            0x55555555514f      0x55555555514f <main+15>
(gdb) x/i $rip
=> 0x55555555514f <main+15>:	mov    DWORD PTR [rbp-0x8],0x0
(gdb) x/3i $rip
=> 0x55555555514f <main+15>:	mov    DWORD PTR [rbp-0x8],0x0
   0x555555555156 <main+22>:	cmp    DWORD PTR [rbp-0x8],0xa
   0x55555555515a <main+26>:	jge    0x555555555175 <main+53>
(gdb) x/7xb $rip
0x55555555514f <main+15>:	0xc7	0x45	0xf8	0x00	0x00	0x00	0x00
(gdb) x/i $rip
=> 0x55555555514f <main+15>:	mov    DWORD PTR [rbp-0x8],0x0


Normally, x/Nb, x/Nw etc. show you fixed size chunks -but instructions aren't fixed size!
some are 1 byte, some are 7 bytes, some are longer.

x/i understands instructions boundaries - it know exactly where one instruction ends and the 
next begins, based on the opcode rules of x86.

Same data, two views - one for humans, one for the machine(raw bytes)
This also exactly what objdump -o does for the entire binary at once -x/i just does it live,
instruction by instruction, while debugging.


You can see the ascii table at - man ascii
