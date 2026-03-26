# CS:APP Quiz - Chapters 7-12

---

## Chapter 7: Linking

**1.** What are the two main tasks performed by a static linker?

A) Compilation and assembly
B) Symbol resolution and relocation
C) Preprocessing and linking
D) Loading and execution

**2.** In an ELF object file, which section contains the machine code of the compiled program?

A) `.data`
B) `.bss`
C) `.text`
D) `.symtab`

**3.** True or False: The `.bss` section occupies actual space in the object file on disk.

**4.** According to the linker's rules for resolving multiply-defined global symbols, which of the following is classified as a **strong** symbol?

A) An uninitialized global variable
B) A function definition
C) A variable declared with `extern`
D) A local static variable

**5.** Consider two files compiled and linked together:

```c
/* foo.c */
int x = 15213;

/* bar.c */
int x;
```

What happens when these are linked?

A) Linker error: duplicate symbol
B) The program uses `x = 15213` from `foo.c`
C) The program uses `x = 0` from `bar.c`
D) Undefined behavior at compile time

**6.** What is a static library (archive) file, and what is its conventional file extension on Unix systems?

**7.** During relocation, the linker encounters a `R_X86_64_PC32` relocation entry. What type of address does this relocation produce?

A) An absolute address
B) A PC-relative address
C) A GOT-relative address
D) A PLT-relative address

**8.** True or False: When using static libraries, the linker only copies the object modules from the archive that are actually referenced by the program.

**9.** What is the primary advantage of dynamic linking with shared libraries (`.so` files) over static linking?

**10.** In Position-Independent Code (PIC), what is the purpose of the Global Offset Table (GOT)?

A) To store the source code of shared libraries
B) To provide an indirection table for referencing global variables whose addresses are not known at compile time
C) To hold the executable's symbol table
D) To manage virtual memory page tables

**11.** In PIC, what is the purpose of the Procedure Linkage Table (PLT)?

**12.** True or False: The order in which static libraries are listed on the linker command line does not matter.

**13.** Which of the following ELF sections contains the string names used in the symbol table?

A) `.symtab`
B) `.rel.text`
C) `.strtab`
D) `.rodata`

**14.** What happens when two strong symbols with the same name are presented to the linker?

A) The linker silently picks one
B) The linker issues an error
C) The linker creates a weak reference
D) The linker merges them into one symbol

**15.** Describe the lazy binding mechanism used by the PLT when a shared library function is called for the first time versus subsequent calls.

---

## Chapter 8: Exceptional Control Flow

**1.** Which class of exception is caused by an external event (e.g., a timer chip or I/O device signaling) and is asynchronous with respect to the processor's instruction execution?

A) Trap
B) Fault
C) Interrupt
D) Abort

**2.** True or False: A trap is intentional and always returns control to the next instruction after the trapping instruction.

**3.** Which of the following is an example of a fault?

A) A system call via `int 0x80`
B) A page fault
C) A divide-by-zero (on x86 this is classified as a fault)
D) Both B and C

**4.** What are the possible return values of `fork()` in the parent and child processes respectively?

A) 0 in both parent and child
B) Child's PID in the parent, 0 in the child
C) 0 in the parent, parent's PID in the child
D) -1 in both on success

**5.** Consider the following code:

```c
int main() {
    fork();
    printf("hello\n");
    fork();
    printf("bye\n");
    return 0;
}
```

How many times is "hello" printed, and how many times is "bye" printed?

**6.** What happens to a child process when it terminates but its parent has not yet called `wait()` or `waitpid()`?

A) It is immediately removed from the system
B) It becomes a zombie process
C) It is adopted by the init process
D) It continues executing in the background

**7.** Which signal is sent to a process when the user presses Ctrl+C at the terminal?

A) SIGCHLD
B) SIGSEGV
C) SIGINT
D) SIGSTOP

**8.** True or False: Signal handlers can be safely written using any standard C library function such as `printf`.

**9.** What is a context switch?

**10.** Which of the following functions is considered async-signal-safe and can be called from within a signal handler?

A) `printf`
B) `malloc`
C) `write`
D) `exit`

**11.** Consider the following code:

```c
int main() {
    int x = 3;
    if (fork() != 0) {
        printf("x=%d\n", ++x);
    }
    printf("x=%d\n", --x);
    return 0;
}
```

What are the possible outputs? (List all lines that will be printed.)

**12.** What does the `exec` family of functions do, and does it return on success?

**13.** True or False: A signal of the same type that is already pending will be queued so that multiple instances are delivered.

**14.** What signal is generated when a child process terminates?

A) SIGINT
B) SIGCHLD
C) SIGKILL
D) SIGSEGV

**15.** What is the difference between `_exit()` and `exit()`?

---

## Chapter 9: Virtual Memory

**1.** In a system using virtual memory, the CPU generates which type of address?

A) Physical address
B) Virtual address
C) Disk address
D) Bus address

**2.** What is the role of the page table in virtual memory?

A) To store the actual data pages in memory
B) To map virtual page numbers (VPN) to physical page numbers (PPN)
C) To manage the CPU's register file
D) To schedule processes for execution

**3.** True or False: A page fault causes the operating system to transfer a page from disk to main memory.

**4.** What does the TLB (Translation Lookaside Buffer) cache?

A) Recently accessed data words
B) Recently used page table entries
C) Disk blocks
D) Instruction opcodes

**5.** Given a virtual address, which bits are used to form the TLBI (TLB Index) and TLBT (TLB Tag)?

A) The physical page number bits
B) The page offset bits
C) Bits from the Virtual Page Number (VPN)
D) Bits from the physical page offset

**6.** Why do systems use multi-level page tables instead of a single-level page table?

**7.** In the context of memory-mapped files, what is Copy-on-Write (COW)?

A) A technique where pages are copied whenever they are read
B) A technique where a shared page is duplicated only when a process attempts to write to it
C) A technique where the OS writes all pages to disk on every context switch
D) A technique where pages are never shared between processes

**8.** True or False: After `fork()`, the parent and child initially share the same physical pages, which are marked as copy-on-write.

**9.** In a `malloc` implementation using an implicit free list, how does the allocator know the size of each block?

**10.** What is coalescing in the context of dynamic memory allocation?

A) Splitting a free block into two smaller blocks
B) Merging adjacent free blocks into a single larger free block
C) Moving allocated blocks to reduce fragmentation
D) Compacting the heap by removing all free blocks

**11.** What is the purpose of boundary tags (footers) in a heap allocator?

**12.** Which of the following is a common memory bug in C programs?

A) Dereferencing a freed pointer (use-after-free)
B) Not freeing allocated memory (memory leak)
C) Writing past the end of an allocated block (buffer overflow)
D) All of the above

**13.** Suppose a system has 14-bit virtual addresses, 12-bit physical addresses, and a page size of 64 bytes. How many bits are in the VPN?

A) 6
B) 8
C) 12
D) 14

**14.** True or False: The `mmap` function can be used to map a file into a process's virtual address space.

**15.** What is internal fragmentation, and how does it differ from external fragmentation?

---

## Chapter 10: System-Level I/O

**1.** In Unix, what are the conventional file descriptor numbers for standard input, standard output, and standard error?

A) 1, 2, 3
B) 0, 1, 2
C) 0, 1, 3
D) 1, 2, 0

**2.** True or False: The `read` system call is guaranteed to return exactly the number of bytes requested.

**3.** What is a "short count" in the context of Unix I/O?

**4.** Which kernel data structure is private to each process and maps file descriptors to entries in the open file table?

A) The v-node table
B) The open file table
C) The descriptor table
D) The inode table

**5.** What happens to the file descriptors after a `fork()` call?

A) The child gets completely independent copies of all open files
B) The child inherits copies of the parent's descriptor table, and both share the same open file table entries
C) All file descriptors in the child are closed
D) Only descriptor 0 is shared; others are duplicated

**6.** What does the following `dup2` call accomplish?

```c
dup2(fd, STDOUT_FILENO);
```

A) Closes `fd`
B) Redirects standard output to the file/resource referred to by `fd`
C) Duplicates standard output into `fd`
D) Opens a new file descriptor

**7.** What is the purpose of the Rio (Robust I/O) package described in CS:APP?

**8.** True or False: Two different processes that open the same file independently will share the same open file table entry in the kernel.

**9.** Which system call is used to open a file and returns a file descriptor?

A) `read`
B) `open`
C) `fopen`
D) `create`

**10.** In the kernel's data structures for I/O, which structure stores metadata about the file itself (such as file size and type), shared among all entries that refer to the same file?

A) Descriptor table
B) Open file table
C) v-node table
D) Buffer cache

**11.** What information does an open file table entry contain?

**12.** True or False: The `close` system call frees the file descriptor and decrements the reference count of the associated open file table entry.

**13.** Consider the following code:

```c
int fd1 = open("file.txt", O_RDONLY);
int fd2 = open("file.txt", O_RDONLY);
```

Do `fd1` and `fd2` share the same open file table entry?

A) Yes, because they refer to the same file
B) No, each `open` call creates a separate open file table entry
C) Only if `file.txt` is a regular file
D) Only if they are in the same process

**14.** After calling `fork()`, the parent closes file descriptor `fd`. What happens to the child's ability to use its copy of `fd`?

**15.** What happens when a process reads from a file that has reached end-of-file (EOF)?

---

## Chapter 11: Network Programming

**1.** In the client-server model, which entity initiates the communication?

A) The server
B) The client
C) The router
D) The DNS server

**2.** True or False: IP addresses are stored in memory in network byte order, which is big-endian.

**3.** What is the role of DNS (Domain Name System)?

A) To encrypt network traffic
B) To map domain names to IP addresses
C) To route packets between networks
D) To manage file transfers

**4.** Which function in the sockets interface is called by a server to assign an address and port to a socket?

A) `socket`
B) `connect`
C) `bind`
D) `accept`

**5.** What is the correct order of socket calls for a typical TCP server?

A) `socket` -> `connect` -> `listen` -> `accept`
B) `socket` -> `bind` -> `listen` -> `accept`
C) `socket` -> `listen` -> `bind` -> `accept`
D) `socket` -> `accept` -> `bind` -> `listen`

**6.** What does the `listen` function do in the sockets interface?

**7.** What is the difference between `connect` (client-side) and `accept` (server-side)?

**8.** An HTTP response begins with a status line. What status code and phrase indicate success?

A) `100 Continue`
B) `200 OK`
C) `301 Moved Permanently`
D) `404 Not Found`

**9.** True or False: An HTTP request line has the format: `method URI version`, for example `GET /index.html HTTP/1.1`.

**10.** What is CGI (Common Gateway Interface) in the context of web servers?

**11.** Which of the following is the correct way to convert a 32-bit integer from host byte order to network byte order?

A) `ntohl()`
B) `htonl()`
C) `ntohs()`
D) `htons()`

**12.** What does the `accept` function return?

A) A new socket file descriptor connected to the client
B) The original listening socket descriptor
C) The client's IP address as a string
D) An error code

**13.** True or False: A socket pair (client IP, client port, server IP, server port) uniquely identifies a TCP connection.

**14.** What HTTP status code indicates that the requested resource was not found on the server?

A) 200
B) 301
C) 403
D) 404

**15.** Describe how a web server uses `fork()` or threads to handle multiple client connections concurrently.

---

## Chapter 12: Concurrent Programming

**1.** Which of the following is NOT one of the three basic approaches to building concurrent programs discussed in CS:APP?

A) Process-based concurrency using `fork()`
B) I/O multiplexing using `select()` or `epoll()`
C) Thread-based concurrency using pthreads
D) Coroutine-based concurrency using `async/await`

**2.** True or False: Threads within the same process share the same virtual address space, including the heap and global variables.

**3.** Which pthread function creates a new thread?

A) `pthread_join`
B) `pthread_create`
C) `pthread_exit`
D) `pthread_detach`

**4.** What is a race condition?

**5.** In the semaphore abstraction used in CS:APP, what do the P() and V() operations do?

A) P() increments the semaphore; V() decrements it
B) P() waits until the semaphore is nonzero then decrements it; V() increments the semaphore
C) P() locks a mutex; V() unlocks it (they are always identical to mutex lock/unlock)
D) P() sends a signal; V() receives a signal

**6.** How is a mutex (binary semaphore) used to protect a critical section?

**7.** Describe the producer-consumer problem and how it is solved using semaphores.

**8.** Which of the following is one of the four classes of thread-unsafe functions described in CS:APP?

A) Functions that return a pointer to a static variable
B) Functions that use only local variables
C) Functions that are reentrant
D) Functions that use mutex locks properly

**9.** True or False: A reentrant function is always thread-safe because it does not reference any shared data.

**10.** What are the four necessary conditions for deadlock to occur?

**11.** What is the simplest strategy to prevent deadlock when a program uses multiple locks?

A) Never use more than one lock
B) Always acquire locks in a consistent, fixed ordering
C) Use recursive locks exclusively
D) Set a timeout on all lock acquisitions

**12.** Consider the following code executed by two threads sharing variable `cnt` (initially 0):

```c
/* Thread routine */
void *thread(void *vargp) {
    for (int i = 0; i < 1000; i++)
        cnt++;
    return NULL;
}
```

If two threads execute this routine concurrently without synchronization, what is the expected final value of `cnt`?

A) Always exactly 2000
B) Always exactly 1000
C) Some value between 0 and 2000, potentially different on each run
D) Always 0

**13.** True or False: In process-based concurrency, the parent and child processes share global variables.

**14.** What is the advantage of I/O multiplexing (e.g., using `select`) over process-based concurrency?

**15.** What does it mean to detach a thread using `pthread_detach`, and why would you do this?
