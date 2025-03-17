# Processes
#cs 

Can be understood in two ways:
- **From the process's POV** 
- From the OS's POV

A process is a running instance of a program; an abstraction created by the OS.

- Each process has its own registers
- Each process has its own view of memory
- Little else is needed
	- Signal state
	- UID, signal mask, whether being debugged

Stack in order (top to bottom):
- Stack
- Heap
- Data
- Text (code)

Heap grows up, Stack grows down.

### Special Registers & Instructions

Stack Pointer
- Points to the top of the stack
- When a new item is pushed to the stack, the pointer adjusts to point to the new top
- Moves down to allocate space for the functions local variable. When the function returns, the stack pointer moves back up (deallocates the space)

Frame Pointer
- Functions create a new frame on the stack, including the function's local variables, arguments, and return address
- Provides stable reference point for accessing function parameters/variables, as the stack pointer changes frequently during function execution

Instruction Pointer
- Stores the address of the next instruction to be executed (increments after each instruction)
- Cannot be read/write to
- Jumps, calls, returns explicitly modify IP

`pushq %rax`:
- Equivalent to:
- `subq $8, %rsp`
- `movq %rax, (%rsp)`
- "Push from rax"

`popq %rax`:
- Equivalent to:
- `movq (%rsp), %rax` 
- `addq $8, %rsp`
- "Pop to rax"

`call 0x12345`:
- Equivalent (pseudo, can't call %rip) to:
- `pushq %rip`
- `movq $0x12345, %rip`

`ret`:
- Equivalent (pseudo, can't call %rip) to:
- `popq %rip`


1. The return address and possibly some of the function's arguments are pushed onto the stack.
2. The Stack Pointer is adjusted to reflect the new top of the stack.
3. The Frame Pointer is then set to point at the start of the new frame, which is typically where the Stack Pointer was before adding local variables.
4. Local variables of the function are then allocated by moving the Stack Pointer further down.

Each process has its own view of memory, containing:

- The `.text` segment of memory used to store the program itself
- The `.data` segment of memory used to store global variables
- The memory used by the heap allocated by `malloc()` 
- The memory used by the stack
The process will think of this memory as a contiguous array ` [text/code | data | heap -->    <--- stack ]`

![[Pasted image 20240126235256.png|500]]

When the function returns, these steps are essentially reversed, with the Stack Pointer and Frame Pointer being reset to their previous values, and the return address popped off the stack to resume execution after the function call.

![[Pasted image 20240126234811.png|400]]


## System Calls

A mechanism for user-level programs to ask the system to do things for it.

Essentially, a syscall is how a computer program requests a service from the kernel of the OS it is executed on. 

Processes are typically not given direct kernel access for security reasons, and syscalls are a way for the OS to regulate access when a process needs it.

Examples:

- `int fd = open(const char* path, int flags)`
- `int rc = write(int fd, const void*, size_t s)`
- `int rc = read(int fd, void*, size_t s)` 

###### How command line works:
```C
while(1){
	write(1,"$ ", 2);
	readcommand(command,args); //parse input
	if((prd==fork())==0) //child?
		{ 
		execute(command, args,0);
		}
	else if(pid > 0) //parent?
	{
		wait(0);
	}else
	{
		perror("failed to fork");
	}
}
```


### Atomic operations

An atomic operation executes without the interruption of any other process in between their execution phase. 

They execute at the lowest level (hence the name) and cant be broken down further.