#### Malware Analysis - Fall 2015
#### Lab 10 Solution

#### Part A
1. N/A

2. The PRCB/PCB takes the form of a KPROCESS struct.

    Some important fields:
    ```
	[ Dispatch header
	[ Pointer to Process page directory
	[ Kernel time
    [ User time
	[ Cycle time
	[ Inswap/Outswap list entry
	[ Thread list head
	[ Process spinlock
	[ Processor affinity
	[ Resident kernel stack count
	[ Ideal node
	[ Process state
    [ Thread seed
	[ Inheritable Thread Scheduling Flags
	```
	Other Fields with less info:
	```
	KGDTENTRY LdtDescriptor
	KIDTENTRY Descriptor
	WORD IopmOffset
	UCHAR Iopl
	ULONG ActiveProcessors
	LIST_ENTRY ReadyListHead
	PVOID VdmTrapcHandler
	CHAR BasePriority
	CHAR QuantumReset
	UCHAR PowerState
	UCHAR Visited
	```

	The PCB allows drivers (and rootkits) to access important processor and operating system info from a convenient data structure.

##### Part B - Lab_10-2.malware

1.  DriverEntry calls the function at 0x401000 which calls 0x401100 which does some more malicious activities.

2. SIDT stores the contents of the interrupt descriptor table into the given operand. LIDT loads from the given operand into the interrupt descriptor table. This can be used to modify the table to hook calls to the IDT to help rootkits do things.

3. The function is spawning threads until every IDT handler has been hooked with the goal function. It does this so that it can hook onto every register.
	
4. The sample hooks the IDT and then has each system call print to the debug info about what cpu it is running on and more. This is not really that malicious, but gives an example of the power a rootkit can have.
