# Sin

Sin is a statically typed, value-oriented programming language built around isolated concurrency.

Ordinary data has value semantics, mutation is explicit, and there are no first-class references. 
Mutable access is temporary and exclusive, with overlapping access rejected at compile time.

Concurrency is based on lightweight isolated processes. 
Each process owns its state and communicates through typed messages, allowing programs to run across multiple CPU cores without shared mutable memory.

Sin provides structs, enums, generics, traits with associated types, and extensions, without classes or inheritance. 
It compiles to native code through a C backend, with no virtual machine required.

## Status

Sin is **experimental** and under active development. 
The compiler, runtime, and language specification are still evolving. 
Isolated concurrency is being implemented, and the first technical preview is planned.

Source code and documentation will be published with the preview.
