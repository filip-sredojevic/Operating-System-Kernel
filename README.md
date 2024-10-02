# Operating System for RISC-V processor

## Project description

This project implements a small operating system kernel for the RISC-V (rv64) architecture, running on top of the `qemu-system-riscv64` emulator. The kernel provides its own dynamic memory allocator (`MemoryAllocator`) that manages the heap through a linked list of free blocks and merges adjacent free segments when memory is released. It implements threads (`TCB`) with their own stack and context, together with a round-robin `Scheduler`, while the context switch itself is written in assembly (`contextSwitch.S`). Thread synchronization is provided by semaphores (`Sem`) with a queue of blocked threads and support for `wait`, `signal`, `tryWait` and closing a semaphore, as well as a `join` operation on threads. Communication between user mode and supervisor mode is based on system calls issued through the `ecall` instruction, with a single trap handler in `Riscv::handleSupervisorTrap()` (`trap.S`) that also handles timer and console interrupts. On top of the C system call interface (`syscall_c.h`) there is a C++ wrapper (`syscall_cpp.h`) with the `Thread`, `Semaphore` and `Console` classes, along with overloaded `new` and `delete` operators.

## Running the project

### Prerequisites

- RISC-V cross-compiler toolchain
- `qemu-system-riscv64`
- `make`

### Setup

This repository contains only the kernel source code (`Headers/` and `Source/`). Building it also requires the base project skeleton (`project_base`), which provides the `lib/` library (`hw.h`, `console.h`), the `Makefile` and the test program (`userMain`). The files from this repository have to be placed into the directory layout the code expects (includes are of the form `../h/...` and `../lib/...`):


### Building and running

```bash
make          # build the kernel and the user program
make qemu     # run the kernel in the QEMU emulator
```

Running an already built kernel manually:

```bash
qemu-system-riscv64 -machine virt -nographic -bios none -kernel kernel
```

To exit QEMU: `Ctrl+A`, then `x`.

