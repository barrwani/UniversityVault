# Virtual Memory
#cs 

Virtual Memory is when the OS builds an abstraction of a private, potentially large address space for multiple running processes (all sharing memory) on top of a single, physical memory.

The running program thinks it is loaded into memory at a particular address (say 0) and has a potentially very large address space (32 or 64 bit).

When a process tries to perform a load at address 0 (a virtual address), the OS, in tandem with hardware support, will have to make sure the load doesn't go to the physical address 0, but rather to the physical address where A is loaded into memory.

![[Pasted image 20240312170947.png|400]]
## Benefits of Virtual Memory

- Programmability 
	- Program thinks it has a lot of memory
	- Programs can use 'easy' addresses: compiler and linker don't have to worry about where program lives in physical memory
	- Multiple instances of a program can be loaded and not collide

- Protection
	- Processes cannot read/write each other memory
	- Enables isolation (which is essential)
- Effective use of resources
	- Don't have to worry that the sum of the memory consumed by all active processes is larger than physical memory.
- Sharing
	- Two processes have a different way to refer to the same physical memory cells


Translation of the virtual and physical addresses is done by the MMU (memory management unit, hardware). 

The OS sets up data structures that the hardware 'sees', associated per-process.

## Paging

Paging is the process of dividing memory (virtual + physical) into fixed-size chunks. 

These chunks are called pages.

They have a size called a page size.

Page Size: x86-64: 4096B = 4KB = $2^{12}$ bytes

Each process has a separate mapping, and each page is separately mapped.

We allow the OS to gain control on certain operations:
- Read-only pages trap to OS on write
- Invalid pages trap to OS on read or write
- OS can change mapping and resume application

Pages have numbers:
```C
page 0:   [0,4095]
page 1:   [4096, 8191]
page 2:   [8192, 12277]
page 3:   [12777, 16384]
page 2^{20}-1 [ ......, 2^{32} - 1]
```

Both virtual and physical pages have numbers, so to be clear use terms like VPN and PPN. 


A dirty page is a page that has been modified and must be rewritten to the disk.
#### VM math

A virtual address is 32-bits, which means that the virtual address space is $2^{32}$ bits = 4GB.

VPN is 20 bits, which means there are $2^{20}$ virtual pages. The offset is 12 bits, which means page size is $2^{12}$ bits = 4KB.

### Page Tables

The page table is conceptually a map from the VPN (Virtual Page Number) to the PPN (Physical Page Number)

![[Pasted image 20240304164753.png|400]]

- Top bits index into page table. Contents at that index are the PPN.
- Bottom bits are the offset, not changed by mapping.
- Physical Address = PPN + Offset

Each page table entry expresses a mapping about a contiguous group of addresses.

Think of it like:
- There is in the sky a 2^36 sized array that maps the virtual address to a physical page
- table\[36-bit virtual page number] = 20-bit physical page number

A page table entry contains:
- 40 bits representing the physical address of the page or the next page table
- Some bits that contain flags and optimization metadata
- Bottom 3 bits, containing:
	- Present/Not Present flag
		- Is the page present in physical memory? If not trigger page fault
	- Read/Write
		- Specifies whether the page can be written to or only read from.
	- User/Kernel
		- Determines the privilege level required to access the page. 
			- Kernel mode allows full access to hardware resources 
			- User mode restricts access to prevent accidental or malicious system damage.

The OS sets these to indicate protection and the hardware enforces them.

#### Multi-level Page Tables

The key idea is to represent the page table as a tree:
- Root node has pointers to other nodes
- Children point to pages
- We map addresses by: 
	- Using the root for uppermost address bits
	- Next level for the next address bits, etc.

The tree is sparse, meaning that many of the child nodes are not filled in. 

We only fill in the parts that are 'in use'

Example:
- Say we want to map 2MB of physical memory at virtual memory of $0,...,2^{21}-1$
- 48 bits: 
	- 9 9 9 9
	- ...
	- bottom one, points to physical pages
- Enormous address space, but few physical resources used, just 512 + 4 physical pages.
- The bottom-most level is a page table. 
- The rest of the structure tells the architecture how to find the page table.

If asked "what piece of address space is described by a given page table entry?":
- Look at how many bits are on the 'left'

#### Helpful reminders

Each entry in the L1 page table corresponds to 512GB of virtual address space

Each entry in the L2 page table corresponds to 1 GB of virtual address space

Each entry in the L3 page table corresponds to 2 MB of virtual address space

Each entry in the L4 page table corresponds to 1 page (4 KB) of virtual address space

So the L4 page table is responsible for translating 2 MB of virtual memory.


Each page table itself consumes 4KB of physical memory
#### Alternatives and Tradeoffs

There are some tradeoffs between:

- Large and small page sizes:
	- Large page sizes means wasting actual memory
	- Small page sizes means lots of page table entries 
		- May or may not get consumed

- Many levels of mapping and few:
	- More levels means less space spent on page structures when address space is sparse (which they nearly always are) 
		- more costly for hardware to walk the page tables
	- Fewer levels means we need to allocate larger pages (which cost more space)
		- the hardware has fewer levels of mapping

### Virtual Memory on `x86-64`

x86 architecture is 64-bits. Registers and addresses are 64-bits wide. 

#### Virtual Addresses

On x86-64 machines, either 57 or 48 bits matter. 

Not all 64-bit patterns correspond to meaningful virtual addresses.

Bit patterns that are valid addresses are called **canonical addresses**.

Assuming a 48-bit address:
	Low canonical addresses range from:
		`0x0000'0000'0000'0000` 
	to:
		`0x0000'7FFF'FFFF'FFFF`.
    High canonical addresses range from:
        `0xFFFF'8000'0000'0000`
    to:
        `0xFFFF'FFFF'FFFF'FFFF`.

#### Physical Addresses

- 52 bits.
- Means a single machine can address up to 4 PB of physical memory.

- If the machine only has 16 GB (say), then physical addresses will (roughly speaking) only have 34 bits that matter
- Thus the top 18 (=52-34) bits of physical addresses will generally be zero

#### Mapping

We have to map 48-bit number (virtual address) to 52-bit number (physical address), at the granularity of ranges of $2^{12}$ 

### Page Table Structures

### TLB

The MMU has to go out to memory on every memory reference (walking the page tables)

The make this fast we need a cache, which is where TLB comes in.

The TLB (Translation Lookaside Buffer) is hardware that stores the recent translations of virtual addresses to physical memory.

The TLB's algorithm works as follows:
- First, extract the VPN from the virtual address and check if the TLB holds the translation for this VPN
	- If it does, we have a TLB hit, which means the TLB holds the translation
		- We can now extract the page frame number (PFN) from the relevant TLB entry, form our desired physical address and access memory.
	- If it doesn't, we have a TLB miss
		- The hardware accesses the page table to find the translation
		- Assuming the virtual memory reference is valid and accessible, it updates the TLB with the translation
		- Once the TLB is updated the hardware retries the instruction.

A page table entry is marked invalid when the page has not been allocated by the process and thus should not be accessed by it. 

If a process attempts to access an invalid page it will be killed. 

This info is stored as a valid bit

A TLB valid bit simply refers to whether or not a TLB entry has a valid translation within it.

A typical TLB Entry contains:
- `VPN | PFN | other bits`
	- Valid bit
	- Protection bits (read/write/execute)
	- Address-space identifier
	- Dirty bit
	- etc...


TLB miss does not imply page fault and vice versa.

### Page Faults


A page fault occurs when an illegal reference is made. 

A reference is illegal, either because it's not mapped in the page tables or because there is a protection violation.

This requires the OS to get involved.

#### Mechanics

The processor constructs a trap frame and transfers execution to an interrupt or trap handler


```C
ss     [stack segment; ignore]
rsp    [former value of stack pointer]
rflags [former value of rflags]
cs     [code segment; ignore]
rip    [instruction that caused the trap]
%rsp --> [error code]
%rip now points to code to handle the trap
```

Error code:
- U/S: user mode fault / supervisor mode fault
- R/W: access was read / access was write
- P: not-present page / protection violation

When page fault happens, the kernel sets up the process's page entries properly, or terminates the process.

#### Uses for Page Faults

Best example is when we overcommit physical memory.

Your program thinks it has 64GB for example, but the hardware only has 16GB. The secondary storage would then be used to store memory pages.
Advantage:
- Address space looks huge
Disadvantage:
- Very slow access to 'paged' memory

- On a page fault, the kernel reads in the faulting page
- The kernel may need to send a page to the disk, which only happens if both:
	- The kernel is out of memory
	- The page that it selects to write out is dirty

Other uses:
- Store memory pages across the network! (Distributed Shared Memory)
	- On page fault, the page fault handler retrieves the page from another machine
- Copy-on-write
	- When creating a copy of a process, copy its page tables instead of its memory, and mark the pages as read-only
	- When a write happens, a page fault results. 
		- At that point, the kernel allocates a new page, copies the memory over, and restarts the user program to do a write
	- Used in `fork()`,`mmap()` etc..
- Accounting
	- To sample what % of memory pages are written to in a time slice
	- Mark a fraction as not present, measure fault frequency
- Demand Paging
- Growing the stack
- BSS page allocation
- Shared text, libraries, memory

#### Costs

What does paging from the disk cost?
$$\text{Average Memory Access Time (AMAT)} = (1-p) * \text{memory access time} + p * \text{page fault time}$$, where $p$ is prob. of page fault.

Page faults are very expensive.



## Page Replacement Policies

Some entity holds a cache of entries and gets a cache miss. The entity now needs to decide which entry to throw away. How does it decide?

FIFO: Eject the oldest page

MIN (OPT): Eject the entry that won't be referenced for the longest time.

FIFO:

	phys_slot    A B C A B D A D B C B
	S1           A     h   D   h   C  
	S2             B     h   A        
	S3               C           B   h

		7 swaps, 4 hits


OPT:

	phys_slot    A B C A B D A D B C B
	S1           A     h     h     C  
	S2             B     h       h   h
	S3               C     D   h      

		5 swaps, 6 hits


LRU: Throw out the least recently used

LRU:

	phys_slot    A B C A B D A D B C B
	S1           A     h     h     C  
	S2             B     h       h   h
	S3               C     D   h      

		5 swaps, 6 hits


But what if our reference string were ABCDABCDABCD?

	phys_slot   A B C D A B C D A B C D 
	 S1         A     D     C     B     
	 S2           B     A     D     C   
	 S3             C     B     A     D 

	 12 swaps, 0 hits...

Do these anomalies always happen?
- No. With policies like LRU, contents of memory with X pages is subset of contents with X+1 pages

#### Implementing LRU

- Reasonable to do in application programs like web servers that cache pages
	- Use a [[Stacks & Queues#Queue|queue]] to track least recently accessed and use a [[Hash Tables|hash map]] to implement (k,v) lookup
- Not so reasonable in an OS as it doubles memory traffic (after every reference you'd have to move some structure to the head of some list)
- In hardware its way too much work to timestamp each reference and keep the list ordered



We can approximate LRU with CLOCK:
- Arrange the slots in a circle
- The 'hand' sweeps around, clearing a bit
- The bit is set when a page is accessed (USE bit/ACCESSED bit)
- Evict a page if the hand points to it when the bit is clear
- Approximates LRU because we're evicting pages that haven't been used in a while


Generalising CLOCK:

- N-th chance
- Don't throw out a page until the hand has swept by N times
- On page fault (need to evict a page), OS looks at where the hand is currently pointing, say some physical page p. 
- Check the USE bit of p.
	- 1-> clear bit and use clear counter
	- 0-> if counter < N keep going, if counter == N replace the page, hasn't been used in a while
- How to pick N
	- Large is a better approximation to LRU
	- Small is more efficient 


Modifications:
- Dirty pages are more expensive to evict
	- Need to be written to disk first to preserve the new data
- Give dirty bits an extra chance before replacing (OS knows which pages are dirty because of DIRTY/MODIFIED bit)
- Common Approach
	- N = 1 for clean pages
	- N = 2 for dirty pages

### Thrashing 

Take an example: Program touches 50 pages equiprobably but only 40 physical frames. 

**Thrashing:** Processes demand more memory for active use than the system has.

3 Reasons:
- Process has no temporal locality (principle that data being accessed now will probably be accessed soon)
- Process has temporal locality but not enough memory
- Individually all processes fit but there's not enough memory


What do we do?

- In the first two reasons, nothing can be done other than restructuring computation or buying memory 
- In third case, shed and load
	- Working set
		- Only run a set of processes such that the union of their working sets fit in memory
		- Working set: the pages a process has touched over some trailing window of time
	- Page fault frequency
		- Track the metric (# page faults / instructions executed)
		- If that rises above a threshold and there is not enough memory on the system swap out the processes
