# CS:APP Quiz - Chapters 1-6

---

## Chapter 1: A Tour of Computer Systems

**1.** What are the four stages of the compilation system, in order, that transform a C source file (`hello.c`) into an executable object file (`hello`)?

A) Compiler, Assembler, Preprocessor, Linker
B) Preprocessor, Compiler, Assembler, Linker
C) Preprocessor, Assembler, Compiler, Linker
D) Compiler, Preprocessor, Linker, Assembler

**2.** Match each compilation stage to the program that performs it and its output. Which row is correct?

A) Preprocessor (cpp) produces hello.s, Compiler (cc1) produces hello.i
B) Preprocessor (cpp) produces hello.i, Compiler (cc1) produces hello.s, Assembler (as) produces hello.o, Linker (ld) produces hello
C) Preprocessor (cpp) produces hello.o, Assembler (as) produces hello.s
D) Compiler (cc1) produces hello.i, Preprocessor (cpp) produces hello.s

**3.** True or False: The assembler (as) translates the assembly-language file (hello.s) into a relocatable object file (hello.o) containing binary machine-language instructions.

**4.** In the typical hardware organization of a system, which bus transfers data between the CPU and main memory?

A) I/O bus
B) System bus and memory bus
C) PCI bus
D) USB bus

**5.** Which of the following correctly lists the memory hierarchy from fastest/smallest to slowest/largest?

A) Disk, Main Memory, L2 Cache, L1 Cache, Registers
B) Registers, L1 Cache, L2 Cache, Main Memory, Disk
C) L1 Cache, Registers, L2 Cache, Disk, Main Memory
D) Registers, L2 Cache, L1 Cache, Main Memory, Disk

**6.** Approximately how many clock cycles does it take the CPU to access data from the L1 cache versus main memory?

A) L1: ~100 cycles; Main memory: ~1 cycle
B) L1: ~4 cycles; Main memory: ~100 cycles
C) L1: ~50 cycles; Main memory: ~500 cycles
D) L1: ~1 cycle; Main memory: ~10 cycles

**7.** The operating system provides three fundamental abstractions. Which of the following correctly names all three?

A) Threads, Virtual Memory, Sockets
B) Processes, Virtual Memory, Files
C) Processes, Caches, Files
D) Virtual Memory, Files, Drivers

**8.** True or False: A process gives each program the illusion that it has exclusive use of the processor, main memory, and I/O devices.

**9.** In the virtual address space layout for a Linux process, which region is located at the very top of the address space (highest addresses)?

A) Stack
B) Heap
C) Kernel virtual memory
D) Read-only code and data

**10.** What is the difference between concurrency and parallelism?

A) They are the same concept with different names
B) Concurrency refers to multiple activities happening simultaneously on different cores; parallelism means interleaving tasks on a single core
C) Concurrency is a general concept of a system with multiple simultaneous activities; parallelism is the use of concurrency to make a system run faster
D) Parallelism is a software concept; concurrency is a hardware concept

**11.** True or False: Context switching between processes is managed entirely by hardware without any operating system involvement.

**12.** Which of the following is NOT one of the forms of parallelism/concurrency discussed in Chapter 1?

A) Thread-level concurrency
B) Instruction-level parallelism
C) Network-level parallelism
D) Single-Instruction, Multiple-Data (SIMD) parallelism

**13.** What role does the linker (ld) play in the compilation system?

A) It translates C code into assembly language
B) It merges relocatable object files and resolves references to produce an executable object file
C) It expands #include directives and macros
D) It converts assembly mnemonics into binary opcodes

**14.** In the virtual address space, the heap grows in which direction and the stack grows in which direction?

A) Heap grows downward; Stack grows upward
B) Both grow upward
C) Heap grows upward; Stack grows downward
D) Both grow downward

**15.** Short Answer: A modern processor can execute multiple instructions per clock cycle, and the instructions don't necessarily execute in the order they appear in the program. What are these two design properties called?

---

## Chapter 2: Representing and Manipulating Information

**1.** Convert the decimal number 250 to hexadecimal.

A) 0xFA
B) 0xEA
C) 0xFB
D) 0xF0

**2.** On a little-endian machine, the 32-bit value `0x01234567` is stored starting at address 0x100. What byte is stored at address 0x100?

A) 0x01
B) 0x23
C) 0x45
D) 0x67

**3.** For a w-bit two's complement representation, what is the range of representable values?

A) -2^w to 2^w - 1
B) -2^(w-1) to 2^(w-1)
C) -2^(w-1) to 2^(w-1) - 1
D) 0 to 2^w - 1

**4.** What is the 8-bit two's complement representation of -5?

A) 10000101
B) 11111011
C) 11111010
D) 10000101

**5.** Compute the bitwise AND: `0x66 & 0x93`. Give your answer in hexadecimal.

A) 0xF5
B) 0x02
C) 0x12
D) 0x97

**6.** True or False: When a signed integer is cast to unsigned in C, the underlying bit pattern does not change; only the interpretation of those bits changes.

**7.** What is the result of the following C expression on a typical 32-bit system?

```c
int x = -1;
unsigned int y = (unsigned int) x;
// What is the value of y?
```

A) 0
B) -1
C) 2147483647 (2^31 - 1)
D) 4294967295 (2^32 - 1)

**8.** In IEEE 754 single-precision floating point, how many bits are allocated to the sign, exponent, and fraction fields respectively?

A) 1, 7, 24
B) 1, 8, 23
C) 1, 10, 21
D) 1, 11, 20

**9.** True or False: Unsigned addition in C can overflow, and when it does, the result wraps around modulo 2^w.

**10.** Consider two 8-bit unsigned numbers: 200 and 100. What is the result of their unsigned addition in 8 bits?

A) 300
B) 44
C) 56
D) 200

**11.** What is the bitwise OR of `0x66 | 0x93`?

A) 0x02
B) 0xF7
C) 0x66
D) 0x93

**12.** In the IEEE 754 representation, what kind of number is represented when the exponent field is all 1s and the fraction field is nonzero?

A) Infinity
B) Zero
C) NaN (Not a Number)
D) A denormalized number

**13.** Short Answer: Given a 4-bit two's complement system, what is the decimal result of computing 5 + 4? Explain what happens.

**14.** What does the following C expression evaluate to?

```c
// Assume 32-bit int
int x = INT_MIN;  // -2147483648
int result = -x;
// What is result?
```

A) 2147483648
B) 2147483647
C) -2147483648
D) 0

**15.** On a big-endian machine, the 32-bit value `0xDEADBEEF` is stored starting at address 0x200. What byte is at address 0x201?

A) 0xDE
B) 0xAD
C) 0xBE
D) 0xEF

---

## Chapter 3: Machine-Level Representation of Programs

**1.** In IA-32, which register traditionally serves as the stack pointer?

A) %eax
B) %ebp
C) %esp
D) %esi

**2.** What value does the following instruction compute and store in `%ecx`?

```asm
leal 4(%eax,%edx,2), %ecx
```

Assume `%eax = 0x100` and `%edx = 0x10`.

A) The value at memory address 0x124
B) 0x124 (without memory access)
C) 0x114
D) 0x140

**3.** What is the general form of an IA-32 memory addressing mode?

A) (Eb + Ei) * s
B) Imm + Eb + Ei * s
C) Imm(Eb, Ei, s) which computes Imm + R[Eb] + R[Ei] * s
D) Imm(Eb) which computes Imm + Eb

**4.** True or False: The `leal` (load effective address) instruction performs a memory read from the computed address.

**5.** Which of the following condition code flags is set when an arithmetic operation produces a result of zero?

A) CF (Carry Flag)
B) ZF (Zero Flag)
C) SF (Sign Flag)
D) OF (Overflow Flag)

**6.** What is the difference between `movl` and `leal`?

A) There is no difference; they are aliases
B) `movl` always moves an immediate value; `leal` always accesses memory
C) `movl` with a memory operand reads from memory; `leal` computes the address without reading memory
D) `leal` can only be used with registers, not memory addresses

**7.** In the IA-32 calling convention, where is the return value of a function typically stored?

A) On the stack
B) In %ecx
C) In %eax
D) In %ebp

**8.** What do the `pushl %ebp` and `movl %esp, %ebp` instructions at the beginning of a function accomplish?

A) They allocate local variables on the stack
B) They save the old frame pointer and set up a new stack frame
C) They restore the caller's stack frame
D) They pass arguments to the function

**9.** The CF (Carry Flag) is set when which of the following occurs?

A) The result is zero
B) The result is negative
C) An unsigned operation overflows (carry out of the most significant bit)
D) A signed operation overflows

**10.** True or False: The `cmpl` instruction computes the difference of its operands and sets condition codes, but does not store the result in the destination.

**11.** Which defense mechanism works by placing a special canary value between the local buffer and the saved frame pointer/return address?

A) Address space layout randomization (ASLR)
B) Stack protector / stack canary
C) Non-executable stack (NX bit)
D) Code signing

**12.** Consider the following assembly code. What high-level C construct does it most likely represent?

```asm
    cmpl $10, %eax
    jge .L2
    addl $1, %eax
    jmp .L1
.L2:
```

A) A switch statement
B) A for/while loop that runs while %eax < 10
C) An if-else statement
D) A function call

**13.** In IA-32, the `ret` instruction performs which action?

A) Pops the return address from the stack and jumps to it
B) Pushes the return address onto the stack
C) Moves the frame pointer to the stack pointer
D) Restores all caller-saved registers

**14.** Short Answer: Explain how a buffer overflow attack works in terms of the stack frame layout, and name two defenses against it.

**15.** What does the following assembly sequence do?

```asm
movl 8(%ebp), %eax
addl 12(%ebp), %eax
```

A) Loads two local variables and adds them
B) Loads the first and second function arguments from the stack frame and adds them
C) Computes a memory address and stores it
D) Pushes two values onto the stack

---

## Chapter 4: Processor Architecture

**1.** The Y86 instruction set is:

A) A commercial instruction set used in Intel processors
B) A simplified instruction set designed for educational purposes based on IA-32
C) An ARM-based instruction set
D) The instruction set used by the x86-64 architecture

**2.** What are the six sequential stages that a Y86 SEQ processor uses to process each instruction?

A) Fetch, Decode, ALU, Memory, Register, PC
B) Fetch, Decode, Execute, Memory, Write Back, PC Update
C) Read, Decode, Compute, Store, Retire, Branch
D) Load, Decode, Execute, Store, Write, Jump

**3.** True or False: In HCL (Hardware Control Language), a boolean expression `a && b` means that signals a and b are simultaneously tested; there is no short-circuit evaluation as in C.

**4.** In a pipelined processor, a data hazard occurs when:

A) Two instructions try to write to memory at the same time
B) A later instruction needs the result of an earlier instruction that has not yet been written back
C) A branch instruction causes the wrong instructions to be fetched
D) An instruction takes too many clock cycles

**5.** What is forwarding (also called bypassing)?

A) Sending the result from an earlier pipeline stage directly to a later stage that needs it, rather than waiting for the write-back stage
B) Predicting the outcome of a branch instruction
C) Skipping pipeline stages for simple instructions
D) Moving instructions forward in the pipeline to fill empty slots

**6.** A control hazard arises from which type of instruction?

A) Arithmetic instructions
B) Memory load instructions
C) Conditional branch and jump instructions
D) Move instructions

**7.** When a data hazard cannot be resolved by forwarding (e.g., a load-use hazard), the processor must:

A) Cancel the instruction
B) Stall the pipeline by inserting a bubble
C) Re-fetch the instruction
D) Raise an exception

**8.** True or False: In the pipelined Y86 (PIPE) processor, each pipeline register holds the intermediate results needed by the next stage.

**9.** What is the primary advantage of pipelining a processor?

A) It makes each individual instruction execute faster
B) It reduces the total number of instructions needed
C) It increases throughput by overlapping the execution of multiple instructions
D) It simplifies the hardware design

**10.** In Y86, the `halt` instruction causes the processor to:

A) Jump to address 0x0
B) Set a status code indicating that the processor should stop
C) Restart execution from the beginning
D) Enter a low-power sleep mode

**11.** Branch prediction with an "always taken" strategy means:

A) The processor never takes branches
B) The processor always predicts that conditional branches will be taken and fetches from the branch target
C) The processor waits until the branch outcome is known
D) The processor alternates between taken and not-taken predictions

**12.** How many pipeline stages does the PIPE processor in CS:APP have?

A) 4
B) 5
C) 6
D) 8

**13.** Short Answer: Explain the difference between stalling and squashing (canceling) in a pipelined processor. When is each used?

**14.** True or False: In the SEQ (sequential) processor design, each instruction completes all of its stages before the next instruction begins.

**15.** In a pipelined processor, what happens when a mispredicted branch is detected?

A) The processor halts
B) The incorrectly fetched instructions are squashed (canceled), and the processor fetches from the correct target
C) The processor completes the wrong instructions and then corrects the results
D) The pipeline is flushed entirely and restarts from the beginning of the program

---

## Chapter 5: Optimizing Program Performance

**1.** What does CPE stand for, and what does it measure?

A) Clocks Per Element; it measures the number of clock cycles used per element in a vector operation
B) Cycles Per Execution; it measures total program execution time
C) Cache Performance Estimate; it measures cache hit rates
D) Compiler Performance Enhancement; it measures compiler optimization level

**2.** True or False: Loop-invariant code motion is an optimization where computations that produce the same result in every loop iteration are moved outside the loop.

**3.** What is the problem with the following C code from a performance perspective?

```c
for (int i = 0; i < strlen(s); i++) {
    // process s[i]
}
```

A) `strlen` is called only once before the loop begins
B) `strlen(s)` is recomputed on every iteration, turning an O(n) loop into O(n^2)
C) The loop index should be unsigned
D) There is no performance problem

**4.** Amdahl's Law states that the overall speedup of a system is limited by:

A) The speed of the processor
B) The fraction of the computation that cannot be improved
C) The number of available cores
D) The memory bandwidth

**5.** According to Amdahl's Law, if 60% of a program can be sped up by a factor of 3, what is the overall speedup?

A) 3.0x
B) 1.5x
C) 1.67x
D) 2.0x

**6.** What is loop unrolling?

A) Converting a for loop into a while loop
B) Removing loop iterations by performing more work per iteration, reducing loop overhead and enabling further optimizations
C) Moving loop-invariant code outside the loop
D) Replacing a loop with recursion

**7.** Why can reducing the number of memory references (using temporary variables) improve performance?

A) It reduces code size
B) The compiler can keep values in registers, which are much faster than memory accesses
C) It reduces the number of instructions
D) It improves branch prediction

**8.** What is a superscalar processor?

A) A processor with a very large cache
B) A processor that can issue and execute multiple instructions per clock cycle
C) A processor that runs at a very high clock frequency
D) A processor that uses multiple levels of cache

**9.** The technique of using multiple accumulators improves performance by:

A) Reducing the number of iterations
B) Breaking a serial dependency chain so that the processor can execute operations in parallel across multiple functional units
C) Using more memory to store intermediate results
D) Increasing the clock frequency

**10.** True or False: Reassociation transformations change the grouping of operations to reduce the length of the critical path of data dependencies.

**11.** What is a critical path in the context of program performance?

A) The most important function in the program
B) The longest chain of data dependencies that determines the minimum execution time
C) The path through memory that is accessed most frequently
D) The control flow path taken most often

**12.** Which of the following is NOT a compiler-friendly optimization a programmer can apply?

A) Reducing procedure calls in inner loops
B) Eliminating unnecessary memory references
C) Rewriting the compiler's register allocation
D) Using loop unrolling

**13.** Short Answer: Explain why a `2x2 loop unrolling with two accumulators` (2x2a) can potentially halve the CPE compared to basic sequential accumulation.

**14.** A write/read dependency (also called a data dependency) prevents the processor from:

A) Fetching the next instruction
B) Executing a later operation until an earlier operation it depends on has completed
C) Writing to the cache
D) Performing branch prediction

**15.** True or False: Modern out-of-order processors can automatically perform some optimizations like register renaming to reduce false dependencies, but they cannot break true data dependencies.

---

## Chapter 6: The Memory Hierarchy

**1.** What is the key difference between SRAM and DRAM?

A) SRAM is slower but cheaper; DRAM is faster but more expensive
B) SRAM is faster and does not need refresh; DRAM is slower and requires periodic refresh
C) SRAM stores data on disk; DRAM stores data in registers
D) There is no significant difference

**2.** The total time to read a sector from a disk consists of three main components. Which answer lists them correctly?

A) Search time, read time, transfer time
B) Seek time, rotational latency, transfer time
C) Access time, decode time, write time
D) Boot time, seek time, spin time

**3.** True or False: Temporal locality means that if a memory location is accessed, nearby memory locations are likely to be accessed soon.

**4.** A cache is organized with parameters S (number of sets), E (number of lines per set), and B (block size in bytes). What is the total cache capacity C?

A) C = S + E + B
B) C = S * E * B
C) C = S * E + B
D) C = S * B / E

**5.** For a cache with S = 4 sets, B = 8 bytes per block, and an address space of 16 bits, how many tag bits, set index bits, and block offset bits are there?

A) Tag: 9, Set index: 4, Offset: 3
B) Tag: 11, Set index: 2, Offset: 3
C) Tag: 10, Set index: 3, Offset: 3
D) Tag: 8, Set index: 5, Offset: 3

**6.** A direct-mapped cache is a cache where:

A) E = 0 (no lines per set)
B) E = 1 (one line per set)
C) S = 1 (one set containing all lines)
D) B = 1 (one byte per block)

**7.** A fully associative cache is a cache where:

A) E = 1 (one line per set)
B) S = 1 (one set containing all lines)
C) There are no tag bits
D) Every access is a hit

**8.** Which type of cache miss occurs when a cache is empty (cold)?

A) Conflict miss
B) Capacity miss
C) Compulsory miss (cold miss)
D) Coherence miss

**9.** True or False: Spatial locality refers to the tendency to access the same data item multiple times in the near future.

**10.** In a write-back cache, when does modified data get written to the next lower level of the hierarchy?

A) Immediately when the write occurs
B) Only when the modified cache line is evicted (replaced)
C) At fixed time intervals
D) When the processor is idle

**11.** What is a conflict miss?

A) A miss that occurs because the cache is completely full
B) A miss that occurs because multiple blocks map to the same cache set and evict each other, even though the cache has unused space
C) A miss that occurs on the first access to a block
D) A miss caused by the write policy

**12.** Which of the following code patterns exhibits good spatial locality?

A) Accessing elements of a 2D array in column-major order when it is stored in row-major order
B) Accessing elements of a 2D array sequentially in row-major order (matching storage layout)
C) Randomly accessing elements of an array
D) Accessing only the first element of each cache line

**13.** What is the difference between a write-through and a write-back cache policy?

A) Write-through caches only handle reads; write-back caches handle both reads and writes
B) Write-through immediately writes to the next level on every write; write-back defers the write until the line is evicted
C) Write-back is always slower than write-through
D) There is no functional difference

**14.** Short Answer: A system has a 32-bit address space and a cache with 64 sets, 4 lines per set (4-way set associative), and 32-byte blocks. Calculate the number of tag bits, set index bits, and block offset bits.

**15.** True or False: Increasing the block size of a cache always improves performance because it exploits more spatial locality.

---

*End of Quiz - Chapters 1 through 6*
