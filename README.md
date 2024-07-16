# from bits to intelligence

the ml stack is massive beyond human comprehension

can we get it down to [100,000 lines](https://supaiku.com/from-bits-to-intelligence)?

this repo contains an (wip) rough outline detailing how that might happen

---

## prerequisites

here are some recommended materials for the difficult stuff

- assembly -- [riscv reader](http://riscvbook.com/)
- compiler -- [crafting interpreters](https://craftinginterpreters.com/)
- os -- [nand2tetris](https://www.nand2tetris.org/)
- tensors, autograd -- [karpathy tutorial](https://www.youtube.com/watch?v=vmj-3s1tku0), [mesozoic-egg's tinygrad notes](https://github.com/mesozoic-egg/tinygrad-notes) and [teenygrad](https://github.com/tinygrad/teenygrad)

---

## components

### 1. hardware

- compute: gpu/dsp style chip, (verilog, 1000)[^1] -- for massively parallel, scalable compute

- host: cpu style chip, verilog (verilog, 1500) -- for os, process management, communication and other traditional computing needs

- memory: mmu (verilog, 1000)

- storage: sd card driver (verilog, 150) -- for simplicity, we can pretend an sd card is all we need for moving data

### 2. software

- c compiler (python, 2000)[^2] -- mostly to get python. needs parser, assembler, linker

- python runtime (c, 50000)[^3] -- mostly to get autograd, but can host the c compiler that it itself runs on

- os (c, 2500) 

- file system: fat (c, 300)

- init, shell, download, cat, editor (c, 500)


#### 3. tensors

- tensorlib: numpy-like (python, 500)

- autograd: (python, 5000)[^4]

#### 4. machine learning

- data processing (500, python)

- gpt 2 (500, python)


---

## how they fit together

#### hardware layer

1. cpu (risc-v)
    - start with a risc-v simulator in a high-level language (e.g., python)
        - start with rv32i
        - write an assembler, hook into simulator
        - extend to support rv64
    - implement register and memory operations
    - develop debugging tools and memory inspectors
    - add support for elf file loading
    - use risc-v tools as your specification
    - port to verilog
    - simulate the verilog implementation using verilator
    - ensure it can accept machine code and elf files

2. gpu/dsp
   - similar to the cpu, but simpler, and more cores
   - implement basic vector operations

3. memory management unit (mmu)
   - implement virtual memory mapping
   - integrate with cpu and gpu designs

4. storage (sd card driver)
   - implement basic read/write operations in verilog
   - interface with the cpu for data transfer

#### software layer

5. c compiler
   - implement in python
   - start with a basic parser for a subset of c
   - develop an assembler for risc-v
   - create a linker to produce elf files
   - test compilation of simple c programs

6. python runtime
   - implement in c
   - start with core language features
   - add support for basic data structures
   - implement a subset of the standard library
   - ensure it can run the c compiler (bootstrapping)

7. operating system
   - implement basic process management
   - develop a simple scheduler
   - implement system calls
   - device drivers for gpu and storage

8. file system (fat)
   - implement on top of the sd card driver
   - develop basic file operations (create, read, write, delete)

9. utilities (init, shell, download, cat, editor)
   - implement these tools in c
   - ensure they interact correctly with the os and file system

#### ml stack

10. tensor library
    - written in python
    - storing and accessing tensors. views, strides, shapes
    - lazily compiled down to riscv assembly
        - start with rv32i, m
        - then support vector instructions

11. autograd
    - written in python
    - start with scalar autograd, micrograd style
    - then support vectors, then tensors
    - combine with tensor library

12. actual machine learning
    - string parsing
    - csv parsing
    - image data parsing

    - write adam 
    - write a feed forward network, train on mnist
    - attention, batchnorm, convolutions(?)
    - integrate with the tensor library and autograd

[^1]: [tiny-gpu](https://github.com/adam-maj/tiny-gpu)

[^2]: you've heard of [co-recursion](https://en.wikipedia.org/wiki/corecursion#:~:text=put%19simply%2c%20corecursive%20algorithms%20use,produce%20further%20bits%20of%20data.), but have you heard of co-[self-hosting](https://en.wikipedia.org/wiki/self-hosting_(compilers))?

[^3]: [micropython](https://github.com/micropython/micropython)

[^4]: [tinygrad](https://github.com/tinygrad/tinygrad)
