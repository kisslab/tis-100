---
layout: page
title: Chapter 2 - Instruction Timings
---

Note: The `MODE` indication on each node shows the node's state from the _previous_ cycle, not the current cycle. For example, all nodes show `IDLE` on the first cycle after startup.

## 1 cycle

- `JMP/JEZ/JNZ/JGZ/JLZ <label>`
- `JRO <literal>`
- `MOV <literal|ACC|NIL> <ACC|NIL>`
- `ADD <literal>`
- `SUB <literal>`
- `NEG`
- `SWP`
- `SAV`
- `NOP`

These instructions always execute in one `RUN` cycle.

## 1+ cycles (blocking I/O reads)

- `JRO <port>`
- `MOV <port> <ACC|NIL>`
- `ADD <port>`
- `SUB <port>`

These instructions block in the `READ` state until a value is available on `port`, then execute in one `RUN` cycle.

## 2+ cycles (blocking I/O writes)

- `MOV <literal|ACC|NIL> <port>`

This instruction places a value on `port`, blocks in the `WRTE` state for at least one cycle, then executes in one `RUN` cycle when the value is consumed by another node.

- `MOV <port1> <port2>`

This instruction blocks until a value is available on `port1`, blocks in the `WRTE` state for at least one cycle, then executes in one `RUN` cycle when the value is consumed by the node on the other end of `port2`.

Writes take two cycles because values are written to _ports_, not to other nodes. The destination node cannot read the value until after the source node has written to the port, and the source node cannot continue execution until the value is read and the port is empty again.

Note: `MOV <port1> <port1>` is perfectly valid and also takes two cycles.

Stack Memory Nodes are the exception to this rule. A Stack Memory Node can execute one write and an unlimited number of reads (in that order) per cycle.

## Halt node execution

### Unconditionally

- `JRO <0|NIL>`
- `<label>: JMP <label>`

### Conditionally

- `JRO ACC`
- `<label>: JEZ/JNZ/JGZ/JLZ <label>`

_Exercise:_ What happens when you `JRO UP/DOWN/LEFT/RIGHT` and the input value is zero? How might that be useful?

### For all nodes

- [REDACTED]
