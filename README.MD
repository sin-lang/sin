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

## A Taste of Sin

```sin

use Sin

// Structs are values, not objects with shared identity.
struct Sale {
    units: Int
    price: Int
}

extension Sale {
    // Properties are always checked for purity.
    def revenue: Int -> this.units * this.price
}

// Purity is opt-in. Higher-order calls stay pure
// when the functions passed to them are pure.
@pure def revenue(_ sales: Array[Sale]): Int {
    return sales.map(s -> s.revenue)
        .reduce(0, combine: (sum, n) -> sum + n)
}

struct Record {
    amount: Int
}

struct Total

// Mutable state belongs to an isolated process.
isolate Ledger {
    total: Int = 0
}

extension Ledger {
    // Only this handler can mutate the process's state.
    mut handle Record(event) {
        this.total += event.amount
    }

    handle Total: Int -> this.total
}

def main() {
    let morning = [Sale(units: 2, price: 15), Sale(units: 1, price: 20)]
    let evening = [Sale(units: 3, price: 10), Sale(units: 1, price: 25)]

    let ledger = spawn Ledger()

    // Processes communicate by sending values, not sharing memory.
    ledger.send(Record(amount: revenue(morning)))
    ledger.send(Record(amount: revenue(evening)))

    // Asking a process returns a Result: the process may have died.
    case await ledger.ask(Total()) {
        .success(let amount) -> print(amount)
        .failure -> print("ledger unavailable")
    }
}
```
