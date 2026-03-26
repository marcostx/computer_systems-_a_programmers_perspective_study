# CS:APP Answers - Chapters 7-12

---

## Chapter 7: Linking

**1.** Answer: B) Symbol resolution and relocation
Explanation: The static linker performs two main tasks: (1) symbol resolution, which associates each symbol reference with exactly one symbol definition, and (2) relocation, which merges separate code and data sections and assigns runtime addresses to every symbol.

**2.** Answer: C) `.text`
Explanation: The `.text` section contains the compiled machine code (instructions) of the program. `.data` holds initialized global/static variables, `.bss` holds uninitialized global/static variables, and `.symtab` holds the symbol table.

**3.** Answer: False
Explanation: The `.bss` section does not occupy actual space in the object file on disk. It is merely a placeholder that records how much space will be needed at runtime. Uninitialized global variables are allocated in memory when the program is loaded, and initialized to zero.

**4.** Answer: B) A function definition
Explanation: Functions and initialized global variables are strong symbols. Uninitialized global variables are weak symbols. The linker rules state: (1) multiple strong symbols with the same name cause an error, (2) given a strong and a weak symbol with the same name the linker picks the strong one, (3) given multiple weak symbols the linker picks any one.

**5.** Answer: B) The program uses `x = 15213` from `foo.c`
Explanation: In `foo.c`, `x` is initialized, making it a strong symbol. In `bar.c`, `x` is uninitialized, making it a weak symbol. Linker rule 2 states that given one strong and one weak symbol of the same name, the linker chooses the strong symbol.

**6.** Answer: A static library (archive) is a collection of concatenated relocatable object files stored in a single file with the `.a` extension (e.g., `libc.a`). It provides a mechanism for the linker to selectively include only those object modules that are referenced by the program, rather than linking every module in the library.

**7.** Answer: B) A PC-relative address
Explanation: `R_X86_64_PC32` tells the linker to compute a 32-bit PC-relative reference. The linker calculates the offset from the current instruction's address to the target symbol's address, allowing position-independent jumps and calls within the same module.

**8.** Answer: True
Explanation: The linker scans the archive and includes only those member object files that resolve currently undefined symbol references. Object modules in the archive that are not needed are simply not copied into the final executable.

**9.** Answer: Dynamic linking with shared libraries avoids code duplication across executables. A single copy of the shared library code resides on disk and can be shared in memory across multiple running processes. This reduces disk space, memory usage, and makes it easy to update a library without recompiling every program that uses it.

**10.** Answer: B) To provide an indirection table for referencing global variables whose addresses are not known at compile time
Explanation: The GOT is a data section that contains entries for each global variable or function referenced by PIC code. Since PIC code cannot contain absolute addresses, it loads addresses indirectly through the GOT, which the dynamic linker fills in at load time.

**11.** Answer: The PLT is a code section that provides a level of indirection for calling external functions in shared libraries. Each PLT entry is a small stub that jumps through a corresponding GOT entry. On the first call the stub invokes the dynamic linker to resolve the function address and update the GOT. Subsequent calls jump directly to the resolved address.

**12.** Answer: False
Explanation: The order matters because the linker processes libraries and object files from left to right on the command line. If library A depends on library B, then A must appear before B on the command line, so that the linker knows which symbols from B are needed by the time it processes B.

**13.** Answer: C) `.strtab`
Explanation: The `.strtab` (string table) section contains the null-terminated strings for symbol names and section names. The `.symtab` section contains the symbol entries themselves, which reference strings in `.strtab` by offset.

**14.** Answer: B) The linker issues an error
Explanation: Linker rule 1 states that multiple strong symbols with the same name are not allowed. If two object files each define a strong symbol with the same name (e.g., two initialized global variables or two functions with the same name), the linker reports a duplicate symbol error.

**15.** Answer: On the first call to a PLT entry, the stub pushes an identifier onto the stack and jumps to the dynamic linker's resolver routine. The resolver looks up the function's actual address, writes it into the corresponding GOT entry, and then jumps to the function. On subsequent calls, the PLT stub jumps through the GOT entry which now points directly to the function, bypassing the resolver. This lazy binding defers resolution costs until the function is actually needed.

---

## Chapter 8: Exceptional Control Flow

**1.** Answer: C) Interrupt
Explanation: Interrupts are caused by external hardware events (I/O devices, timers) and are asynchronous to the program's instruction stream. Traps and faults are synchronous exceptions caused by executing an instruction. Aborts are unrecoverable errors.

**2.** Answer: True
Explanation: A trap is a deliberate exception, typically used to implement system calls. After the trap handler finishes, control returns to the instruction immediately following the trapping instruction (the next instruction), unlike faults which re-execute the faulting instruction.

**3.** Answer: D) Both B and C
Explanation: Faults are potentially recoverable errors caused by the current instruction. Page faults and divide-by-zero are both classified as faults on x86. If the fault handler can correct the condition (e.g., load a page from disk), the faulting instruction is re-executed. If not (e.g., divide-by-zero), the process is typically terminated.

**4.** Answer: B) Child's PID in the parent, 0 in the child
Explanation: `fork()` returns the child's PID (a positive integer) to the parent process, and returns 0 to the newly created child process. A return value of -1 indicates an error.

**5.** Answer: "hello" is printed 2 times; "bye" is printed 4 times.
Explanation: After the first `fork()`, there are 2 processes. Each prints "hello" (2 total). Then each process calls `fork()` again, creating 4 processes total. Each prints "bye" (4 total).

**6.** Answer: B) It becomes a zombie process
Explanation: A terminated process that has not been reaped by its parent becomes a zombie. It retains its PID and exit status in the kernel's process table so the parent can eventually retrieve this information via `wait`/`waitpid`. Zombies consume minimal resources but do hold a process table entry.

**7.** Answer: C) SIGINT
Explanation: Pressing Ctrl+C at the terminal sends the SIGINT (interrupt) signal to every process in the foreground process group. The default action for SIGINT is to terminate the process.

**8.** Answer: False
Explanation: Signal handlers should only call async-signal-safe functions. `printf` is NOT async-signal-safe because it uses global data structures (stdio buffers) and locks that may already be held when the signal interrupts the main program, leading to deadlocks or data corruption. Use `write` instead.

**9.** Answer: A context switch is the mechanism by which the operating system saves the state (registers, program counter, kernel data structures) of the currently running process and restores the saved state of a different process, transferring control to that process. Context switches are triggered by system calls, interrupts, or when the OS scheduler decides to preempt the current process.

**10.** Answer: C) `write`
Explanation: `write` is one of the functions guaranteed to be async-signal-safe by POSIX. It does not manipulate global data structures like `printf` (which uses stdio buffers) or `malloc` (which manipulates the heap). Signal handlers should only call async-signal-safe functions.

**11.** Answer: The parent (where `fork() != 0` is true) prints `x=4` then `x=3`. The child (where `fork() != 0` is false) prints `x=2`. So the output lines are: `x=4`, `x=3`, and `x=2` (the interleaving of parent and child lines is nondeterministic, but the parent always prints `x=4` before `x=3`).
Explanation: Parent: enters if-block, increments x to 4, prints "x=4", falls through to decrement x to 3, prints "x=3". Child: skips if-block, decrements x from 3 to 2, prints "x=2". Each process has its own copy of `x` (initially 3).

**12.** Answer: The `exec` family of functions (`execve`, `execvp`, etc.) replaces the current process's memory image with a new program loaded from an executable file. It initializes the stack, heap, and code/data segments from the new program and begins execution at the new program's entry point. `exec` does NOT return on success; it only returns if an error occurs (e.g., the executable file is not found).

**13.** Answer: False
Explanation: Unix signals are not queued. If a signal of the same type is already pending (has been sent but not yet received), additional signals of that type are simply discarded. This means a process can receive fewer signals than were sent.

**14.** Answer: B) SIGCHLD
Explanation: When a child process terminates (or stops), the kernel sends a SIGCHLD signal to the parent process. The parent can install a handler for SIGCHLD to reap child processes by calling `waitpid`.

**15.** Answer: `_exit()` terminates the process immediately at the kernel level without flushing stdio buffers, calling `atexit` handlers, or performing any other cleanup. `exit()` is a C library function that calls `atexit`-registered cleanup handlers, flushes all open stdio streams, then calls `_exit()`. In signal handlers, `_exit()` is preferred because it is async-signal-safe, while `exit()` is not.

---

## Chapter 9: Virtual Memory

**1.** Answer: B) Virtual address
Explanation: When virtual memory is enabled, the CPU generates virtual addresses. The MMU (Memory Management Unit) translates these virtual addresses to physical addresses using the page table before accessing main memory.

**2.** Answer: B) To map virtual page numbers (VPN) to physical page numbers (PPN)
Explanation: The page table is an array of page table entries (PTEs) indexed by the virtual page number. Each PTE contains the physical page number (if the page is resident in memory) along with permission and status bits (valid, dirty, read/write).

**3.** Answer: True
Explanation: A page fault occurs when a program accesses a virtual page that is not currently in physical memory. The page fault handler in the OS selects a victim page (evicting it to disk if dirty), loads the requested page from disk into the freed physical frame, updates the page table, and restarts the faulting instruction.

**4.** Answer: B) Recently used page table entries
Explanation: The TLB is a small, fast hardware cache within the MMU that stores recently accessed page table entries (VPN-to-PPN mappings). It allows the MMU to translate virtual addresses without accessing the page table in memory on every reference.

**5.** Answer: C) Bits from the Virtual Page Number (VPN)
Explanation: The VPN is split into a TLB Tag (TLBT, the higher-order bits of the VPN) and a TLB Index (TLBI, the lower-order bits of the VPN). The TLBI selects a set in the TLB, and the TLBT is compared against tags in that set to find a match.

**6.** Answer: A single-level page table for a large address space would be enormous (e.g., a 48-bit virtual address space with 4 KB pages would require 2^36 entries). Multi-level page tables solve this by using a hierarchical structure: if a large region of the virtual address space is unmapped, the corresponding first-level PTE is null, and the second-level page table for that region does not need to exist at all. This dramatically reduces the memory required to store the page table for sparse address spaces.

**7.** Answer: B) A technique where a shared page is duplicated only when a process attempts to write to it
Explanation: Copy-on-write allows multiple processes (e.g., parent and child after `fork()`) to share the same physical pages as long as they only read from them. The pages are marked read-only. When a process tries to write, a page fault occurs, and the OS creates a private copy of that page for the writing process.

**8.** Answer: True
Explanation: After `fork()`, the child's page table entries point to the same physical pages as the parent's. These shared pages are marked as copy-on-write (read-only). Only when either process writes to a page does the kernel create a private copy, avoiding the cost of copying the entire address space.

**9.** Answer: Each block in an implicit free list has a header (typically one word) that encodes the block size and an allocated/free bit. The allocator traverses the list by using the size field in each header to compute the address of the next block. There are no explicit next/previous pointers; the structure is implicit in the contiguous layout and size fields.

**10.** Answer: B) Merging adjacent free blocks into a single larger free block
Explanation: Coalescing is the process of merging adjacent free blocks when a block is freed. Without coalescing, the heap would suffer from false fragmentation: plenty of free memory exists but is split across many small non-contiguous free blocks, making it impossible to satisfy large allocation requests.

**11.** Answer: Boundary tags (footers) are replicas of the header placed at the end of each block. They allow the allocator to determine the size and allocation status of the previous block in constant time by examining the word just before the current block. This enables efficient backward coalescing when a block is freed and the previous block is also free, without needing to traverse the entire list.

**12.** Answer: D) All of the above
Explanation: All three are classic C memory bugs. Dereferencing freed pointers (use-after-free), failing to free allocated memory (memory leaks), and writing past the end of allocated blocks (buffer overflows) are all common and dangerous memory errors.

**13.** Answer: B) 8
Explanation: Page size = 64 bytes = 2^6, so the page offset is 6 bits. Virtual address = 14 bits. VPN = 14 - 6 = 8 bits. This means there are 2^8 = 256 virtual pages.

**14.** Answer: True
Explanation: The `mmap` system call creates a mapping between a region of the process's virtual address space and a file (or anonymous memory). This allows the program to access file contents as if they were in memory, with the OS handling page faults to load file data on demand.

**15.** Answer: Internal fragmentation occurs when the allocated block is larger than the payload requested, wasting space inside the allocated block (e.g., due to alignment requirements or minimum block size constraints). External fragmentation occurs when there is enough total free memory to satisfy a request, but no single contiguous free block is large enough. Internal fragmentation depends only on the previous pattern of requests; external fragmentation also depends on future requests, making it harder to predict and prevent.

---

## Chapter 10: System-Level I/O

**1.** Answer: B) 0, 1, 2
Explanation: By convention, file descriptor 0 is standard input (stdin), 1 is standard output (stdout), and 2 is standard error (stderr). These are automatically opened when a process starts via the shell.

**2.** Answer: False
Explanation: The `read` system call can return fewer bytes than requested (a "short count"). This can happen when reading from a terminal (returns after one line), reading from a network socket (returns whatever data is available), or when the file has fewer remaining bytes than requested.

**3.** Answer: A short count occurs when the `read` or `write` system call transfers fewer bytes than the application requested. This is not an error; it can happen with network sockets (data arrives in chunks), terminals (line-buffered input), and near end-of-file. Applications must handle short counts by looping until all desired bytes have been transferred.

**4.** Answer: C) The descriptor table
Explanation: Each process has its own descriptor table, which is an array indexed by file descriptor number. Each entry points to an entry in the kernel's open file table. The open file table and v-node table are shared by all processes.

**5.** Answer: B) The child inherits copies of the parent's descriptor table, and both share the same open file table entries
Explanation: After `fork()`, the child gets a copy of the parent's descriptor table. Each corresponding entry in parent and child points to the same open file table entry, meaning they share the file position (offset). The reference count in each open file table entry is incremented.

**6.** Answer: B) Redirects standard output to the file/resource referred to by `fd`
Explanation: `dup2(fd, STDOUT_FILENO)` copies the descriptor table entry for `fd` to the entry for file descriptor 1 (stdout). After this call, any writes to stdout go to whatever file or resource `fd` refers to. If descriptor 1 was previously open, it is silently closed first.

**7.** Answer: The Rio (Robust I/O) package provides wrapper functions that handle short counts automatically. It offers unbuffered transfer functions (`rio_readn`, `rio_writen`) that loop until the requested number of bytes is transferred, and buffered read functions (`rio_readlineb`, `rio_readnb`) that efficiently read text lines and binary data from files and sockets.

**8.** Answer: False
Explanation: Each `open` call creates a new, separate open file table entry, even if two different processes (or the same process) open the same file. Each open file table entry has its own file position. However, they will point to the same v-node table entry (same underlying file).

**9.** Answer: B) `open`
Explanation: The `open` system call opens a file specified by a pathname and returns a small non-negative integer (the file descriptor) that identifies the open file in subsequent I/O operations. `fopen` is a C standard library function (not a system call) that returns a `FILE*` stream pointer.

**10.** Answer: C) v-node table
Explanation: The v-node table contains one entry per distinct open file. It stores file metadata such as file type, file size, and access permissions, as well as a pointer to the inode on disk. Multiple open file table entries can point to the same v-node entry.

**11.** Answer: An open file table entry contains: the current file position (offset), a reference count (number of descriptor table entries pointing to it), and a pointer to the corresponding v-node table entry. The file position is updated on each read/write and can be set with `lseek`.

**12.** Answer: True
Explanation: When `close(fd)` is called, the kernel frees the descriptor table entry for `fd` (making it available for reuse) and decrements the reference count of the associated open file table entry. If the reference count drops to zero, the open file table entry is deleted, and the v-node reference count is also decremented.

**13.** Answer: B) No, each `open` call creates a separate open file table entry
Explanation: Even though both calls open the same file, each `open` call creates a distinct open file table entry with its own file position. Both entries point to the same v-node. This means `fd1` and `fd2` maintain independent file positions.

**14.** Answer: The child can still use its copy of `fd` normally. After `fork()`, parent and child have separate descriptor tables that initially point to the same open file table entries. When the parent calls `close(fd)`, it only frees its own descriptor table entry and decrements the reference count. Since the child still references the open file table entry, the reference count remains positive and the entry persists.

**15.** Answer: When a `read` encounters the end of the file (no more data to read), it returns 0 (a return value of 0 bytes transferred). This is how applications detect EOF. It is distinct from an error, which is indicated by a return value of -1.

---

## Chapter 11: Network Programming

**1.** Answer: B) The client
Explanation: In the client-server model, the client always initiates the transaction by sending a request to the server. The server listens for incoming connections and responds to client requests.

**2.** Answer: True
Explanation: IP addresses are stored in network byte order, which is big-endian (most significant byte at the lowest memory address). Functions like `htonl` and `ntohl` convert between host byte order (which may be little-endian on x86) and network byte order.

**3.** Answer: B) To map domain names to IP addresses
Explanation: DNS is a distributed database that translates human-readable domain names (e.g., `www.example.com`) into IP addresses (e.g., `93.184.216.34`) that are used by the network protocols to route packets.

**4.** Answer: C) `bind`
Explanation: The `bind` function associates a socket with a specific local IP address and port number. A server calls `bind` to specify the address/port on which it will listen for incoming client connections.

**5.** Answer: B) `socket` -> `bind` -> `listen` -> `accept`
Explanation: A TCP server creates a socket, binds it to an address/port, calls `listen` to mark it as a passive (listening) socket and set the backlog queue size, then calls `accept` to wait for and accept incoming client connections.

**6.** Answer: The `listen` function converts an active socket (created by `socket`) into a passive listening socket that can accept incoming connection requests. It also specifies the backlog, which is the maximum number of pending connections that can be queued while the server is busy handling other connections.

**7.** Answer: `connect` is called by the client and initiates a TCP connection to a specified server address and port. It blocks until the connection is established (the three-way handshake completes). `accept` is called by the server on a listening socket and blocks until a client connects. It then returns a new connected socket descriptor specifically for communicating with that client, leaving the original listening socket available for more connections.

**8.** Answer: B) `200 OK`
Explanation: HTTP status code 200 with the reason phrase "OK" indicates that the request was successful and the server is returning the requested content in the response body.

**9.** Answer: True
Explanation: An HTTP request line consists of three parts separated by spaces: the method (e.g., GET, POST), the URI (e.g., /index.html), and the HTTP version (e.g., HTTP/1.1), terminated by a carriage return and line feed (`\r\n`).

**10.** Answer: CGI (Common Gateway Interface) is a standard protocol for a web server to execute an external program (a CGI program) and return its output as the response to an HTTP request. When the server receives a request for a URL mapped to a CGI program, it forks a child process, sets environment variables (e.g., QUERY_STRING, CONTENT_LENGTH), and the CGI program writes its output to stdout, which the server sends back to the client.

**11.** Answer: B) `htonl()`
Explanation: `htonl()` converts a 32-bit integer from host byte order to network byte order. `ntohl()` does the reverse. `htons()` and `ntohs()` are the 16-bit equivalents used for port numbers.

**12.** Answer: A) A new socket file descriptor connected to the client
Explanation: `accept` extracts the first pending connection from the listen queue and returns a new connected socket descriptor that can be used to communicate with the client. The original listening socket remains open and can accept more connections.

**13.** Answer: True
Explanation: A TCP connection is uniquely identified by the 4-tuple: (client IP address, client port, server IP address, server port). This allows a server to maintain multiple simultaneous connections with different clients (or even the same client on different ports).

**14.** Answer: D) 404
Explanation: HTTP status code 404 (Not Found) indicates that the server could not find the resource identified by the request URI. 200 is success, 301 is a redirect, and 403 is forbidden.

**15.** Answer: A concurrent web server handles multiple clients by creating a new process (via `fork()`) or a new thread for each incoming client connection. When a client connects via `accept`, the server forks a child process (or spawns a thread) that handles all communication with that client on the connected socket, while the parent (or main thread) loops back to `accept` and waits for the next client. In the process-based approach, the parent closes its copy of the connected descriptor and the child closes its copy of the listening descriptor.

---

## Chapter 12: Concurrent Programming

**1.** Answer: D) Coroutine-based concurrency using `async/await`
Explanation: CS:APP covers three approaches to concurrent programming: (1) process-based using `fork()`, (2) I/O multiplexing using `select()`, and (3) thread-based using pthreads. Coroutine-based concurrency (async/await) is a higher-level paradigm not covered in the book.

**2.** Answer: True
Explanation: Threads within the same process share the entire virtual address space, including the code, global variables, heap, and shared library code/data. Each thread has its own thread ID, stack, stack pointer, program counter, and registers, but the address space itself is shared.

**3.** Answer: B) `pthread_create`
Explanation: `pthread_create` creates a new thread that begins executing a specified routine. `pthread_join` waits for a thread to terminate. `pthread_exit` terminates the calling thread. `pthread_detach` marks a thread so its resources are automatically freed upon termination.

**4.** Answer: A race condition occurs when the correctness of a program depends on the specific scheduling/interleaving of operations across concurrent threads or processes. The outcome is nondeterministic and depends on which thread runs when. A common example is when a thread reads a shared variable that another thread is updating; the result depends on timing.

**5.** Answer: B) P() waits until the semaphore is nonzero then decrements it; V() increments the semaphore
Explanation: P (from Dutch "proberen" = test) checks if the semaphore value s is nonzero; if so, it decrements s and returns. If s is zero, P blocks until s becomes nonzero. V (from Dutch "verhogen" = increment) increments s, potentially waking a thread blocked on P. Both P and V are atomic operations.

**6.** Answer: A mutex (binary semaphore initialized to 1) protects a critical section as follows: a thread calls P() (lock) before entering the critical section and V() (unlock) after leaving it. Since the semaphore starts at 1, the first thread to call P() succeeds (decrementing to 0) and enters the critical section. Any other thread calling P() will block (because the value is 0) until the first thread calls V() (incrementing back to 1). This guarantees mutual exclusion.

**7.** Answer: The producer-consumer problem involves a shared, bounded buffer between producer threads (which generate items and insert them into the buffer) and consumer threads (which remove items from the buffer and process them). It is solved using three semaphores: a mutex (initialized to 1) for mutual exclusion on buffer access, a "slots" semaphore (initialized to the buffer capacity n) counting empty slots, and an "items" semaphore (initialized to 0) counting available items. A producer calls P(slots), P(mutex), inserts an item, V(mutex), V(items). A consumer calls P(items), P(mutex), removes an item, V(mutex), V(slots).

**8.** Answer: A) Functions that return a pointer to a static variable
Explanation: The four classes of thread-unsafe functions in CS:APP are: (1) functions that fail to protect shared variables, (2) functions that keep state across multiple invocations (using static variables), (3) functions that return a pointer to a static variable (e.g., `ctime`, `gethostbyname`), and (4) functions that call thread-unsafe functions.

**9.** Answer: True
Explanation: A reentrant function accesses only local variables on the stack and does not reference any shared global or static data. Since each thread has its own stack, reentrant functions are inherently thread-safe without needing synchronization. Reentrant functions are a proper subset of thread-safe functions.

**10.** Answer: The four necessary conditions for deadlock (Coffman conditions) are: (1) **Mutual exclusion** -- at least one resource is held in a non-shareable mode, (2) **Hold and wait** -- a thread holds at least one resource while waiting to acquire another, (3) **No preemption** -- resources cannot be forcibly taken from a thread; they must be released voluntarily, (4) **Circular wait** -- a circular chain of threads exists where each thread holds a resource that the next thread in the chain is waiting for. All four conditions must hold simultaneously for deadlock to occur.

**11.** Answer: B) Always acquire locks in a consistent, fixed ordering
Explanation: If all threads acquire locks in the same global order (e.g., always lock A before lock B), then circular wait is impossible, which breaks one of the four necessary conditions for deadlock. This is called the lock ordering rule and is the simplest and most commonly used deadlock prevention strategy.

**12.** Answer: C) Some value between 0 and 2000, potentially different on each run
Explanation: The `cnt++` operation is not atomic; it consists of load, increment, and store steps. Without synchronization, the two threads can interleave these steps, causing lost updates (one thread's increment overwrites another's). The theoretical range is from 2 (maximum interference) to 2000 (no interference), and the actual result varies nondeterministically between runs.

**13.** Answer: False
Explanation: After `fork()`, the parent and child processes have completely separate virtual address spaces. Changes to global variables in one process are not visible to the other. This is a key difference from threads, which share global variables within the same address space.

**14.** Answer: I/O multiplexing allows a single process to monitor multiple file descriptors simultaneously and respond to whichever one is ready. Advantages over process-based concurrency include: (1) it gives the programmer full control over scheduling, (2) all "flows" share the same address space so sharing data is easy and does not require IPC, (3) it avoids the overhead of creating and context-switching between processes, and (4) it is easier to debug since there is a single process.

**15.** Answer: `pthread_detach` marks a thread as detached, meaning its resources (stack, thread descriptor) will be automatically reclaimed by the system when the thread terminates, without requiring another thread to call `pthread_join`. You detach a thread when you do not need its return value and do not want to (or cannot) explicitly join it. This prevents resource leaks from un-joined threads. A thread can also detach itself by calling `pthread_detach(pthread_self())`.
