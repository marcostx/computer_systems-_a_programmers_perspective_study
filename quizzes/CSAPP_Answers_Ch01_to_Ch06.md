# CS:APP Answers - Chapters 1-6

---

## Chapter 1: A Tour of Computer Systems

**1.** Answer: B
Explanation: The four stages are Preprocessor (cpp), Compiler (cc1), Assembler (as), and Linker (ld), in that order. The preprocessor handles #include and macros, the compiler generates assembly, the assembler produces machine code in an object file, and the linker combines object files into an executable.

**2.** Answer: B
Explanation: The preprocessor (cpp) expands macros and includes to produce hello.i (modified source). The compiler (cc1) translates that to assembly hello.s. The assembler (as) converts assembly to a relocatable object file hello.o. The linker (ld) combines object files to produce the final executable hello.

**3.** Answer: True
Explanation: The assembler translates human-readable assembly language (.s) into a relocatable object file (.o) containing binary machine-language instructions packaged in the ELF format.

**4.** Answer: B
Explanation: The CPU communicates with main memory via the system bus (connecting CPU to the I/O bridge) and the memory bus (connecting the I/O bridge to main memory). The I/O bus connects to peripheral devices like disks and graphics adapters.

**5.** Answer: B
Explanation: The hierarchy from fastest/smallest to slowest/largest is: Registers, L1 Cache, L2 Cache (and L3 if present), Main Memory (DRAM), Local Disk, Remote Storage. Each level serves as a cache for the level below it.

**6.** Answer: B
Explanation: L1 cache access takes roughly 4 clock cycles, while main memory access takes approximately 100 cycles. This large gap is why the cache hierarchy exists -- to bridge the processor-memory speed gap.

**7.** Answer: B
Explanation: The three fundamental OS abstractions are: processes (abstraction for the processor), virtual memory (abstraction for main memory), and files (abstraction for I/O devices).

**8.** Answer: True
Explanation: A process is the OS abstraction that gives each running program the illusion of exclusive use of the hardware. The OS achieves this through context switching (processor), virtual memory (memory), and the file system (I/O).

**9.** Answer: C
Explanation: The kernel virtual memory resides at the top of the virtual address space (highest addresses). Below it, from high to low, are the stack, shared libraries, the heap, read/write data, and read-only code/data at the bottom.

**10.** Answer: C
Explanation: Concurrency is the general concept of a system with multiple simultaneous activities. Parallelism refers to using concurrency to make a system run faster. Concurrency can exist without true parallelism (e.g., time-slicing on a single core).

**11.** Answer: False
Explanation: Context switching is managed by the operating system kernel. The OS saves the state (context) of the current process and restores the state of the next process to run. Hardware provides support mechanisms (like timer interrupts), but the OS orchestrates the switch.

**12.** Answer: C
Explanation: Chapter 1 discusses thread-level concurrency, instruction-level parallelism, and SIMD parallelism. Network-level parallelism is not one of the forms discussed.

**13.** Answer: B
Explanation: The linker (ld) merges multiple relocatable object files (e.g., hello.o and library files like printf.o), resolves symbol references between them, and produces a single executable object file that can be loaded into memory and run.

**14.** Answer: C
Explanation: In the Linux virtual address space layout, the heap starts just above the read/write data segment and grows upward (toward higher addresses). The stack starts below the kernel memory region and grows downward (toward lower addresses).

**15.** Answer: The two properties are superscalar execution (issuing multiple instructions per cycle) and out-of-order execution (executing instructions in a different order than they appear in the program, based on data availability rather than program order).

---

## Chapter 2: Representing and Manipulating Information

**1.** Answer: A
Explanation: 250 in decimal = 15 * 16 + 10 = FA in hex. 15 = F, 10 = A, so 250 = 0xFA.

**2.** Answer: D
Explanation: In little-endian byte order, the least significant byte is stored at the lowest address. For 0x01234567, the bytes from least significant to most significant are: 67, 45, 23, 01. So address 0x100 contains 0x67, 0x101 contains 0x45, 0x102 contains 0x23, and 0x103 contains 0x01.

**3.** Answer: C
Explanation: In w-bit two's complement, the most negative value is -2^(w-1) (the sign bit alone) and the most positive value is 2^(w-1) - 1 (all bits set except the sign bit). The range is asymmetric because zero uses one of the non-negative bit patterns.

**4.** Answer: B
Explanation: To find -5 in 8-bit two's complement: start with +5 = 00000101, invert all bits to get 11111010, then add 1 to get 11111011. So -5 = 0xFB = 11111011 in binary.

**5.** Answer: B
Explanation: 0x66 = 0110 0110 in binary, 0x93 = 1001 0011 in binary. AND operation: 0110 0110 & 1001 0011 = 0000 0010 = 0x02.

**6.** Answer: True
Explanation: In C, casting between signed and unsigned types of the same size does not change the bit pattern. The same bits are simply reinterpreted under unsigned or two's complement rules. For example, -1 (all 1-bits as signed) becomes the maximum unsigned value.

**7.** Answer: D
Explanation: The value -1 in 32-bit two's complement is represented as all 1-bits (0xFFFFFFFF). When cast to unsigned, the same bit pattern is interpreted as 2^32 - 1 = 4294967295.

**8.** Answer: B
Explanation: IEEE 754 single-precision (32-bit) float has 1 sign bit, 8 exponent bits (with a bias of 127), and 23 fraction bits (with an implicit leading 1 for normalized values).

**9.** Answer: True
Explanation: Unsigned addition of two w-bit values can produce a result that requires w+1 bits. Since only w bits are stored, the result wraps around modulo 2^w. For example, in 8-bit unsigned: 200 + 100 = 300, but 300 mod 256 = 44.

**10.** Answer: B
Explanation: 200 + 100 = 300, but the maximum 8-bit unsigned value is 255. The result wraps: 300 - 256 = 44. This is unsigned overflow with modular arithmetic.

**11.** Answer: B
Explanation: 0x66 = 0110 0110, 0x93 = 1001 0011. OR operation: 0110 0110 | 1001 0011 = 1111 0111 = 0xF7.

**12.** Answer: C
Explanation: In IEEE 754, when the exponent field is all 1s and the fraction field is nonzero, the value represents NaN (Not a Number). If the fraction field were zero, it would represent positive or negative infinity (depending on the sign bit).

**13.** Answer: In a 4-bit two's complement system, the range is -8 to +7. The value 5 (0101) + 4 (0100) = 9, but 9 exceeds the maximum representable value of 7. The binary result is 1001, which in two's complement is -7. This is positive overflow: the sum of two positive numbers wraps around to a negative number.

**14.** Answer: C
Explanation: INT_MIN in 32-bit two's complement is -2^31 = -2147483648. Its negation, +2147483648, cannot be represented in 32-bit two's complement (max positive is 2^31 - 1). Due to overflow, -INT_MIN wraps back to INT_MIN, so the result is -2147483648.

**15.** Answer: B
Explanation: On a big-endian machine, the most significant byte is stored at the lowest address. For 0xDEADBEEF: address 0x200 = 0xDE, 0x201 = 0xAD, 0x202 = 0xBE, 0x203 = 0xEF. So address 0x201 contains 0xAD.

---

## Chapter 3: Machine-Level Representation of Programs

**1.** Answer: C
Explanation: In IA-32, %esp is the stack pointer register. It points to the top of the stack (the lowest address in the current stack frame). %ebp is the base/frame pointer.

**2.** Answer: B
Explanation: `leal` computes the address but does NOT access memory. The computation is: 4 + %eax + %edx * 2 = 4 + 0x100 + 0x10 * 2 = 4 + 256 + 32 = 292 = 0x124. This value (0x124) is placed directly into %ecx.

**3.** Answer: C
Explanation: The general IA-32 addressing mode is Imm(Eb, Ei, s), which computes the effective address as Imm + R[Eb] + R[Ei] * s. Here Imm is an immediate offset, Eb is the base register, Ei is the index register, and s is a scale factor (1, 2, 4, or 8).

**4.** Answer: False
Explanation: `leal` (Load Effective Address) computes the address using the addressing mode formula but does NOT read from memory. It simply stores the computed address value into the destination register. This is why it is frequently used for arithmetic.

**5.** Answer: B
Explanation: ZF (Zero Flag) is set to 1 when the result of an arithmetic or logical operation is zero. CF flags unsigned overflow, SF flags a negative result, and OF flags signed overflow.

**6.** Answer: C
Explanation: `movl` with a memory operand as the source reads from that memory address and places the value into the destination. `leal` computes the address using the same addressing mode arithmetic but places the address itself into the destination without accessing memory. This makes `leal` useful for quick arithmetic.

**7.** Answer: C
Explanation: The IA-32 calling convention specifies that the return value of a function is placed in the %eax register. The caller reads %eax after the function returns.

**8.** Answer: B
Explanation: `pushl %ebp` saves the caller's frame pointer onto the stack. `movl %esp, %ebp` sets the current stack pointer as the new frame pointer. Together, they establish a new stack frame for the called function while preserving the ability to restore the caller's frame.

**9.** Answer: C
Explanation: The Carry Flag (CF) is set when an unsigned arithmetic operation produces a carry out of (or borrow into) the most significant bit. It indicates unsigned overflow. The Overflow Flag (OF) indicates signed overflow.

**10.** Answer: True
Explanation: `cmpl` (compare) computes S2 - S1 (note the reversed order in AT&T syntax) and sets the condition code flags (CF, ZF, SF, OF) based on the result, but the result itself is discarded. It is used purely to set flags for subsequent conditional instructions.

**11.** Answer: B
Explanation: The stack protector (stack canary) places a random guard value between local buffers and the saved frame pointer / return address. Before the function returns, it checks whether the canary value has been modified. If so, the program aborts, preventing a buffer overflow from hijacking the return address.

**12.** Answer: B
Explanation: The code compares %eax with 10, jumps past the loop body if %eax >= 10, otherwise increments %eax and jumps back. This is a loop pattern equivalent to `while (eax < 10) { eax++; }`.

**13.** Answer: A
Explanation: The `ret` instruction pops the top value from the stack (the return address that was pushed by the corresponding `call` instruction) into the program counter (%eip), effectively jumping back to the instruction after the call site.

**14.** Answer: A buffer overflow attack works by writing data beyond the bounds of a local buffer on the stack, overwriting the saved return address with the address of malicious code (or a ROP gadget chain). When the function returns, it jumps to the attacker-controlled address instead of the legitimate caller. Two defenses are: (1) Stack canaries -- a random guard value placed before the return address that is checked before returning; if overwritten, the program aborts. (2) Address Space Layout Randomization (ASLR) -- randomizes the addresses of the stack, heap, and libraries each time the program runs, making it difficult for attackers to predict where to jump. Other valid defenses include non-executable stack (NX/DEP).

**15.** Answer: B
Explanation: In the standard IA-32 calling convention, `8(%ebp)` is the first argument to the function, and `12(%ebp)` is the second argument. The code loads the first argument into %eax, then adds the second argument to it. This is equivalent to `return arg1 + arg2`.

---

## Chapter 4: Processor Architecture

**1.** Answer: B
Explanation: Y86 is a simplified instruction set designed by the CS:APP authors for educational purposes. It is based on a subset of IA-32 and is used to teach processor design concepts including sequential and pipelined implementations.

**2.** Answer: B
Explanation: The six stages of the SEQ processor are: Fetch (read instruction from memory), Decode (read registers), Execute (ALU operation), Memory (read/write data memory), Write Back (write results to registers), and PC Update (determine next PC value).

**3.** Answer: True
Explanation: In HCL, boolean expressions describe combinational logic where all inputs are evaluated simultaneously by hardware circuits. There is no sequential short-circuit evaluation as in C -- all signal values are determined by the physical circuit in parallel.

**4.** Answer: B
Explanation: A data hazard occurs when an instruction depends on the result of a preceding instruction that has not yet completed its write-back stage. The dependent instruction needs a value that is still being computed somewhere in the pipeline.

**5.** Answer: A
Explanation: Forwarding (bypassing) allows a result computed in an earlier pipeline stage (e.g., the execute or memory stage) to be sent directly to a later stage that needs it, without waiting for the value to be written to the register file in the write-back stage. This eliminates many data hazard stalls.

**6.** Answer: C
Explanation: Control hazards arise from conditional branch and jump instructions because the processor does not know the correct next instruction address until the branch condition is evaluated. This can cause incorrect instructions to enter the pipeline.

**7.** Answer: B
Explanation: A load-use hazard occurs when an instruction needs a value that is being loaded from memory by the immediately preceding instruction. Since the data is not available until after the memory stage, forwarding alone cannot resolve it. The processor must stall (insert a bubble) for one cycle to wait for the data.

**8.** Answer: True
Explanation: Pipeline registers sit between each pair of adjacent stages and hold all intermediate values, signals, and status information needed by the next stage. They allow each stage to work on a different instruction simultaneously.

**9.** Answer: C
Explanation: Pipelining increases throughput by allowing multiple instructions to be in different stages of execution simultaneously. While each individual instruction still takes the same total time (or slightly more due to pipeline overhead), the overall rate at which instructions complete is much higher.

**10.** Answer: B
Explanation: The `halt` instruction sets the processor status to HLT, indicating that the processor should stop executing instructions. In the Y86 simulator, this terminates the program.

**11.** Answer: B
Explanation: The "always taken" branch prediction strategy assumes every conditional branch will be taken and immediately begins fetching instructions from the branch target address. If the prediction is wrong, the incorrectly fetched instructions must be squashed.

**12.** Answer: B
Explanation: The PIPE processor in CS:APP has 5 pipeline stages: Fetch, Decode, Execute, Memory, and Write Back. The PC Update logic from SEQ is folded into the Fetch stage of the pipeline.

**13.** Answer: Stalling holds an instruction in its current pipeline stage for one or more cycles by inserting a bubble (nop) into the next stage. It is used for data hazards (like load-use hazards) where a value is not yet available. Squashing (canceling) removes incorrectly fetched instructions from the pipeline by replacing them with bubbles. It is used for control hazards when a branch misprediction is detected -- the instructions that were speculatively fetched from the wrong path are canceled and the correct instructions are fetched.

**14.** Answer: True
Explanation: In the SEQ design, the processor completes all six stages (Fetch, Decode, Execute, Memory, Write Back, PC Update) for one instruction before beginning the next. This makes it conceptually simple but slow, as only one instruction is in progress at a time.

**15.** Answer: B
Explanation: When a mispredicted branch is detected (typically in the Execute stage), the instructions that were incorrectly fetched and partially processed are squashed -- replaced with bubbles so they have no effect. The processor then begins fetching from the correct branch target address.

---

## Chapter 5: Optimizing Program Performance

**1.** Answer: A
Explanation: CPE stands for Cycles Per Element. It measures the number of clock cycles the processor uses per element when performing a vector or array operation. Lower CPE means better performance. It provides a more meaningful metric than total execution time for comparing implementations.

**2.** Answer: True
Explanation: Loop-invariant code motion identifies expressions inside a loop that compute the same value on every iteration and moves them before the loop. This avoids redundant recomputation. For example, moving `strlen(s)` outside the loop when the string does not change.

**3.** Answer: B
Explanation: `strlen(s)` is called in the loop condition on every iteration. Since `strlen` scans the entire string (O(n)) to find the null terminator, calling it n times makes the loop O(n^2). The fix is to compute the length once before the loop.

**4.** Answer: B
Explanation: Amdahl's Law states: Speedup = 1 / ((1 - f) + f/S), where f is the fraction that can be improved and S is the speedup of that fraction. The overall speedup is limited by (1 - f), the fraction that cannot be improved. Even infinite speedup of the improvable part gives only 1/(1-f) overall.

**5.** Answer: C
Explanation: Using Amdahl's Law: Speedup = 1 / ((1 - 0.6) + 0.6/3) = 1 / (0.4 + 0.2) = 1 / 0.6 = 1.667x. Even though 60% of the program runs 3x faster, the overall speedup is only about 1.67x because the remaining 40% is unchanged.

**6.** Answer: B
Explanation: Loop unrolling processes multiple elements per iteration, reducing loop overhead (fewer branch tests and counter updates) and exposing more instruction-level parallelism. For example, a 2x unrolled loop processes 2 elements per iteration instead of 1.

**7.** Answer: B
Explanation: Memory references go through the cache/memory hierarchy, which is slower than registers. By using temporary variables for frequently accessed values, the compiler can keep them in CPU registers. This eliminates redundant loads and stores, significantly improving performance in tight loops.

**8.** Answer: B
Explanation: A superscalar processor has multiple functional units (ALUs, multipliers, load/store units) and can issue, dispatch, and complete more than one instruction per clock cycle. This exploits instruction-level parallelism.

**9.** Answer: B
Explanation: A single accumulator creates a serial dependency chain: each addition depends on the previous result. Multiple accumulators split the computation into independent partial sums that can execute in parallel across the processor's multiple functional units, then combine at the end.

**10.** Answer: True
Explanation: Reassociation changes how operations are grouped (e.g., `(a*b)*c` to `a*(b*c)`) to break dependency chains. This can reduce the critical path length, allowing the processor to overlap more operations. The result may differ slightly for floating point due to rounding.

**11.** Answer: B
Explanation: The critical path is the longest sequence of operations where each depends on the result of the previous one. It determines the minimum possible execution time regardless of how many functional units are available, because those operations must execute sequentially.

**12.** Answer: C
Explanation: Register allocation is done by the compiler, not the programmer. Programmers can help by reducing procedure calls, eliminating unnecessary memory references, and applying loop unrolling. The compiler handles mapping variables to registers.

**13.** Answer: With basic sequential accumulation, there is a single dependency chain: each multiply-add depends on the previous result, so the CPE is bounded by the latency of the multiply (or add) operation. With 2x2 unrolling and two accumulators, the work is split into two independent chains (e.g., even-indexed and odd-indexed elements). Each chain has half the operations, and both chains can execute in parallel on separate functional units. The CPE is potentially halved because the throughput bound (number of operations divided by available functional units) becomes the bottleneck instead of the latency bound.

**14.** Answer: B
Explanation: A write/read (true data) dependency means a later instruction reads a value produced by an earlier instruction. The later instruction cannot execute until the earlier instruction's result is available. This serializes execution along the dependency chain.

**15.** Answer: True
Explanation: Out-of-order processors use register renaming to eliminate false dependencies (write-after-write and write-after-read hazards), allowing more instructions to execute in parallel. However, true data dependencies (read-after-write) are fundamental: if instruction B needs the result of instruction A, B must wait for A regardless of hardware optimizations.

---

## Chapter 6: The Memory Hierarchy

**1.** Answer: B
Explanation: SRAM (Static RAM) uses a bistable circuit (typically 6 transistors per bit) that holds its value as long as power is applied, with no refresh needed. DRAM (Dynamic RAM) stores each bit as charge on a capacitor, which leaks and must be periodically refreshed. SRAM is faster (~1-10 ns) but more expensive and less dense; DRAM is slower (~50-100 ns) but cheaper and denser.

**2.** Answer: B
Explanation: Disk access time = seek time (moving the head to the correct track) + rotational latency (waiting for the desired sector to rotate under the head) + transfer time (reading the sector data as it passes under the head). Seek time and rotational latency dominate; transfer time is relatively small.

**3.** Answer: False
Explanation: The description given is spatial locality, not temporal locality. Temporal locality means that if a memory location is accessed, the same location is likely to be accessed again in the near future. Spatial locality means that nearby locations are likely to be accessed soon.

**4.** Answer: B
Explanation: Total cache capacity C = S * E * B. S sets, each containing E lines, each line holding B bytes of data. For example, a cache with S=64 sets, E=4 lines/set, B=32 bytes/block has C = 64 * 4 * 32 = 8192 bytes = 8 KB (not counting tag and valid bit storage).

**5.** Answer: B
Explanation: With S = 4 sets: set index bits = log2(4) = 2 bits. With B = 8 bytes: block offset bits = log2(8) = 3 bits. With a 16-bit address: tag bits = 16 - 2 - 3 = 11 bits. The address is partitioned as [11 tag bits | 2 set index bits | 3 offset bits].

**6.** Answer: B
Explanation: A direct-mapped cache has exactly one line per set (E = 1). Each memory block maps to exactly one cache line determined by the set index bits. This is the simplest cache organization but is most susceptible to conflict misses.

**7.** Answer: B
Explanation: A fully associative cache has a single set (S = 1) containing all E cache lines. Any memory block can be placed in any line. There are no set index bits; the entire address (minus offset bits) is the tag. This eliminates conflict misses but requires searching all lines on every access.

**8.** Answer: C
Explanation: A compulsory (cold) miss occurs when a cache line is accessed for the first time, when the cache is empty or has never contained that block. These misses are unavoidable regardless of cache size or associativity. Capacity misses occur when the working set exceeds cache size. Conflict misses occur due to set mapping collisions.

**9.** Answer: False
Explanation: The description given is temporal locality. Spatial locality is the tendency to access memory locations near a recently accessed location. For example, sequentially iterating through an array exhibits spatial locality; accessing the same variable in a loop exhibits temporal locality.

**10.** Answer: B
Explanation: In a write-back policy, writes only update the cache line (marking it as dirty). The modified data is written to the next lower level of the hierarchy only when the dirty line must be evicted to make room for a new block. This reduces memory bus traffic compared to write-through but requires a dirty bit per line.

**11.** Answer: B
Explanation: A conflict miss occurs when a cache has unused capacity, but multiple blocks that the program accesses map to the same set and keep evicting each other. For example, in a direct-mapped cache, two arrays whose addresses map to the same set will thrash even if 99% of the cache is empty.

**12.** Answer: B
Explanation: Accessing a 2D array in row-major order (matching C's storage layout) accesses consecutive memory addresses, exhibiting excellent spatial locality. Each cache line fetched contains multiple useful elements. Column-major access strides through memory with large gaps, missing opportunities to use cached data.

**13.** Answer: B
Explanation: Write-through writes data to both the cache and the next lower level of memory on every write, ensuring consistency at the cost of higher memory bus traffic. Write-back only writes to the cache, deferring the write-through to the next level until the line is evicted. Write-back reduces traffic but adds complexity (dirty bits, eviction writes).

**14.** Answer: Block offset bits = log2(32) = 5 bits. Set index bits = log2(64) = 6 bits. Tag bits = 32 - 6 - 5 = 21 bits. The cache has 64 sets, each 4-way set associative with 32-byte blocks. Total capacity = 64 * 4 * 32 = 8192 bytes = 8 KB. The 32-bit address is partitioned as: [21 tag bits | 6 set index bits | 5 block offset bits].

**15.** Answer: False
Explanation: Increasing block size improves spatial locality exploitation up to a point, but beyond that it hurts performance. Larger blocks mean fewer total lines in the cache (for fixed cache size), increasing conflict and capacity misses. Also, the penalty for a miss increases because more data must be transferred. There is an optimal block size that balances spatial locality benefits against these costs.

---

*End of Answers - Chapters 1 through 6*
