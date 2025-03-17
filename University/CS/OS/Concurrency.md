# Concurrency
#cs 

## Threads & Forks

#### Forks

The `fork()` syscall creates a new [[Processes|process]]. It requires no arguments.

#### Threads

Threads are used due to parallelism, essentially allowing a process to be run on multiple CPUs (cores) to make them run faster on modern hardware. 

The process of parallelization involves taking a single-threaded process and making it multi-threaded.

It is also useful to avoid blocking program process due to slow I/O. While one thread in the process waits (blocked waiting for I/O), the CPU scheduler can switch to other threads that are ready to run. 

Threading enables overlap of I/O with other activities within a single program, much like multiprogramming did for processes across programs.


- `create(func, NULL)` - Creates a new thread which runs `func`
- `join(threadid)` - Waits for a thread to complete before continuing

## Managing Concurrency

**A Critical Section** is a piece of code that accesses a shared resource, usually a variable or data structure

**A Race condition** arises if multiple threads of execution enter the critical section at the same time, both attempt to update the shared data structure, leading to unwanted behaviour.

**An Indeterminate Program** consists of one or more race conditions, meaning the output of the program varies from run to run. The output is thus not **deterministic**. 

To avoid these problems, threads should use some kind of **mutual exclusion** primitives, guaranteeing that only a single thread ever enters a critical section, thus avoiding a race condition.

A set of instructions are **atomic** if they can not be interrupted. They either **all run** or do **not run at all**.

#### Critical Sections

Critical sections protect from concurrent execution:
- **Mutual exclusion** 
- Progress
	- If no thread is executing a critical section, let another thread in
- Bounded waiting
	- There is a bound on the number of threads that can enter a critical section before a thread.

Protecting critical sections:
- `lock()/unlock()`
- `enter()/leave()`
- `acquire()/release()`


Implementing Critical Sections for a single CPU machine:
- `enter()` to disable interrupts
- `leave()` to enable interrupts


### Monitors
#### Mutex

Mutex are mutually-exclusive variables. Critical sections surrounded by mutexes can only be executed one thread at a time. A single mutex can be used to wrap multiple critical sections.

Once a thread has entered a critical section via `mutex.acquire()`, invariants (shared resources) can be violated, and access to them becomes mutually excusive to the thread.

When a thread exits a critical section via `mutex.release()`, access shared resources is restored.

### Condition Variables

Condition variables are not actually variables. Think of it like a queue, where the thread stops its executing once it enters the queue if a specific condition is not met.

The two main condition variable operations:
- `wait()`
	- pauses thread and waits to be woken
- `signal()`
	- Wakes one waiting thread
- `broadcast()`
	- Wakes all waiting threads

A monitor is one mutex + one or more condition variables
#### Spinlocks

If a lock is not available, the thread employs busy-waiting (spinning), where it continuously checks until the lock becomes available.
Other locking mechanisms that might put the thread to sleep while it waits for the lock to be released.

### Deadlocks

A deadlock occurs when all four of these conditions are present:
- Mutual exclusion
- Hold and wait
- No preemption
- Circular wait

What can be done about deadlock:
- Ignore it
- Detect and recover
- Avoid algorithmically
- Negate one of the four conditions
- Static/dynamic detection tools

### Performance issues and tradeoffs

- Invoking spinlocks/mutexes can be expensive
- Coarse locks limit parallelism 
- Fine-grained locks lead to complexity and hence bugs

Programmability issues:
- Loss of modularity

## Rules of Concurrent Programming

- Acquire/release at the beginning/end of a method or a function
- Hold lock when doing Condition Variable operations
- A thread in `wait()` must be prepared to be restarted at any time, not just when another thread calls `signal()`
	- `while(not safe to proceed){wait();}`
- Don't call `sleep()`

### Steps to take

1. Getting Started
	- Identify units of concurrency 
	- Identify chunks of state
	- Write down high-level main loop of each thread
	- Separate threads from objects
2.  Write down the synchronization constraints and the kind (mutual exclusion or scheduling)
3. Create a lock or CV for each constraint 
4. Write the methods, using the locks and CVs


