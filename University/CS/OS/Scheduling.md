# Scheduling
#cs 

### Scheduling Decisions

The OS has to decide which process (or thread) to run.

Scheduling decisions happen when a process/thread:
- Switches from running to waiting
- Switches from running to ready
- Switches from waiting to ready
- Exits

#### Preemptive scheduling vs non-preemptive scheduling

Preemptive scheduling is used when a process: 
- switches from the running state to the ready state
- switches from the waiting state to the ready state

Resources are allocated to the process for a limited amount of time and then taken away

The process is placed back in the ready queue if that process still has CPU burst time remaining. 

That process stays in the ready queue till it gets its next chance to execute.

Non-preemptive scheduling is used when:
- A process terminates
- A process switches from running to waiting state.

**Preempting**: When we **preempt** a job, we run it partially then place it back on the queue to allow other jobs to run. 

**Burst Time:** Execution time. The amount of CPU time the process requires to complete execution.

#### Metrics and Criteria

- Turnaround time: time for each process/thread to complete 
- Waiting/response/output time: time spent waiting for something to happen
- System throughput: # of completed processes / time
- Fairness: Often conflicts with efficiency

#### Context switching 

Context switching has a cost:
- CPU time in kernel (save and restore registers, switch address spaces)
- Indirect costs (TLB shootdowns, processor cache, OS caches)

### Scheduling Disciplines

- Assume first that the process/threads do no I/O (unrealistic, we relax it later)

##### FCFS (first come first served)/FIFO
- Run job until its done
- Example:
	- P1 needs 24s, P2 needs 3s, P3 needs 3s
	- $\frac{\text{3 processes}}{24+3+3} = \frac{\text{3 processes}}{30s} = 0.1 \text{proc/sec}$ throughput
	- $\frac{24+27+30}{3}= 27s$ avg time taken
- Very high average turnaround
  
- **Advantages:**
	- Simple
	- No starvation
	- Few context switches
- **Disadvantage:**
	- Short jobs get stuck behind long ones

![[Pasted image 20240312015912.png]]


#### SJF (Shortest Job First) / STCF (Shortest Time to Completion First)
##### SJF: Choose job with shortest upcoming CPU burst
  ![[Pasted image 20240312015925.png]]
##### STCF: preemptive version of SJF
- We preempt our longer job until a shorter job arrives, then let those jobs run and place the rest of the longer job at the end of the queue 
  
- **Advantages:**
	- Low Disk utilization
	- Optimal average turnaround
	- Low overhead
- **Disadvantages:**
	- Long running jobs can get starved
	- Does not optimize response time, only turnaround time
	- Requires future prediction
	  
![[Pasted image 20240312015750.png]]
#### RR (Round Robin) 
- Instead of running jobs to completion, run for a time slice (scheduling quantum)
- After each quantum we switch to the next job and rotate.

- **Advantages:**
	- Fair allocation of CPU across jobs
	- Low avg response time when job lengths vary
	- Good for output time if small number of jobs
- **Disadvantages**
	- What if jobs are same length?
	- Example:
		- 2 jobs of 50 time units each, quantum is 1
		- Average time taken: 99.5 quanta
		- With FCFS: 75 quanta

![[Pasted image 20240312015516.png]]

#### Picking Quantum Size

- Want it to be much larger than context switch cost
- Majority of bursts should be less than the quantum
- Too small and spend too much time context switching
- Too large and response time suffers
- Typically 10-100ms

### Incorporating I/O

A scheduler has a decision to make when a job initiates an I/O request, because the currently running job won't be using the CPU during I/O

- It is blocked waiting for I/O completion.

It also has a decision to make when the I/O is completed. 

When that occurs, a system interrupt is raised and the OS runs and moves the process that issued the I/O from blocked back to the ready state.

In the below example, A runs for 10ms then issues an I/O request that takes 10ms, and B takes 50ms with no I/O.  

We run A first as it is shorter (STCF), then while waiting for A's I/O we run a slice of B

![[Pasted image 20240312020655.png]]


##### Disk Utilization

To calculate disk utilization for a given job:

$$\text{disk util }=\frac{\text{disk time}}{\text{disk time + CPU time}}$$

### Priority Scheme

Give every process a number (set by admin) and give the CPU to the process with the highest priority (lowest or highest number depending on scheme). 

Can be done preemptively or non-preemptively.

It is generally a bad idea to use strict priority because of starvation of low priority tasks.

The solution to this starvation is to increase a process's priority as it waits.
### MLFQ

The multi-level feedback queue divides processes into multiple queues based on their priority levels. 

There are three main ideas:

 - Multiple Queues: Each with a different priority
 - Round Robin within each queue: flush a priority level before moving on to the next one
 - Feedback: Process' priority changes, eg: based on how little or much it's used in the CPU
##### Basic Rules:

- Rule 1: If Priority(A) > Priority(B), A runs
- Rule 2: if Priority(A) = Priority(B), A&B run in Round Robin

**Advantages:**
- Approximates SRTCF (shortest remaining time to completion)

**Disadvantages:**
- Can't donate priority
- Not very flexible
- Not good for real-time or multimedia
- Can be gameable: user puts in meaningless I/O to keep job prio high
### Lottery and Stride Scheduling

#### Lottery Scheduling

- Issue lottery tickets to processes
	- Let $p_i$ have $t_i$ tickets
	- $T$ = total # of tickets, $T = \sum t_i$ 
	- Chance of winning next quantum is $\frac {t_i} T$ 
	- Tickets not used up

Controls long-term average proportion of CPU for each process.

It can also group processes hierarchically for control:
- Subdivide lottery tickets
- Can model as currency, so there can be an exchange rate between real currency and lottery tickets

**Advantages:**
- Deals with starvation
	- If you have one ticket you will make progress)
- Adding one high-prio job will not starve the others
- Adding/Deleting jobs affects all jobs proportionally
- Can transfer tickets between processes
	- If a client is waiting for a server it can donate tickets to the server so it can run
- Ticket inflation for processes that don't use their whole quantum


**Disadvantages:**
- Latency unpredictable
- High expected error


#### Stride Scheduling

Stride scheduling was made to address these disadvantages

Each job in the system has a stride, which is inverse in proportion to the number of tickets it has. 

We call this value the stride of each process; every time a process runs, we will increment a counter for it (called its pass value) by its stride to track its global progress.

### Linux : CFS (Completely Fair Scheduler)

Implements fair-share scheduling using a form of stride scheduling, a deterministic fair-share scheduler.

The goal is simple: to fairly divide a CPU evenly among all competing processes. 

It does so through a simple counting-based technique known as virtual runtime (`vruntime`).

In the most basic case, each process’s `vruntime` increases at the same rate, in proportion with physical (real) time. 

When a scheduling decision occurs, CFS will pick the process with the lowest `vruntime` to run next.

### Scheduling Lessons

Scheduling comes up all over the place
- m requests share n resources
- Disk: which read/write request to do next?
- Memory: which process to take physical page from?

In the modern day, scheduling problems are not so relevant when you can upgrade to a faster CPU/faster network.

Here are some exceptions:
- Websites and large-scale networks often cannot be made fast enough to handle peak demand so scheduling becomes important again
- Some scheduling decisions have non-linear effects on the systems behaviour overall, not just different performance for different users
- Real-time systems
	-  soft real time: miss deadline and CD or MPEG decode will skip
	- hard real time: miss deadline and plane will crash

Scheduling decisions shouldn't effect a programs results in principle
- Its rare to be able to calculate the best scheduling
- Instead, we build the kernel so that it's correct to context switch and restore at any time, so then any schedule will get the right answer for a program

Cases where scheduling can affect correctness:
- Multimedia: delay too long and the results will look/sound wrong
- Web server: delay too long and users give up

### Three Main Lessons

1. Know your goals, write them down
2.  Compare against optimal, even if optimal can't be built
3. There are actually many different schedulers in the system that interact:
	- Mutexes implicitly make scheduling decisions
	- Interrrupts likewise...
	- Disk scheduler doesn't know how to favour one process' I/O over another
	- Network scheduler doesn't know how to favour one process' packets over another
	- etc..