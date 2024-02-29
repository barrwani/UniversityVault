#cs 

# Concurrency

## Threads & Forks

#### Forks

The `fork()` syscall creates a new [[Processes|process]]. It requires no arguments.

#### Threads

Threads are used due to parallelism, essentially allowing a process to be run on multiple CPUs (cores) to make them run faster on modern hardware. 

The process of parallelization involves taking a single-threaded process and making it multi-threaded.

It is also useful to avoid blocking program process due to slow I/O. While one thread in the process waits (blocked waiting for I/O), the CPU scheduler can switch to other threads that are ready to run. 

Threading enables overlap of I/O with other activities within a single program, much like multiprogramming did for processes across programs.


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


#### Mutex

Mutex are mutually-exclusive variables. Critical sections surrounded by mutexes can only be executed one thread at a time. A single mutex can be used to wrap multiple critical sections.

Once a thread has entered a critical section via `mutex.acquire()`, invariants (shared resources) can be violated.

### Condition Variables

Condition variables are not actually variables. Think of it like a queue, where the thread stops its executing once it enters the queue if a specific condition is not met.

The two main condition variable operations:
- `wait()`
	- .
- `signal()`
	- .
- `broadcast()`
	- .


#### Rules of Concurrent Programming

- Acquire/release at the beginning/end of a method or a function
- Hold lock when doing Condition Variable operations
- A thread in `wait()` must be prepared to be restarted at any time, not just when another thread calls `signal()`
	- `while(not safe to proceed){wait();}`
- Don't call `sleep()`