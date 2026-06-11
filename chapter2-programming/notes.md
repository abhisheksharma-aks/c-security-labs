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


