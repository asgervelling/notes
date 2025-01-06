Main article: [[RISC-V]].

## Register Names

The RISC-V registers have names and designated uses.

| Register | ABI Name | Description                       | Saver  |
| -------- | -------- | --------------------------------- | ------ |
| x0       | zero     | Hard-wired zero                   |        |
| x1       | ra       | Return address                    | Caller |
| x2       | sp       | Stack pointer                     | Callee |
| x3       | gp       | Global pointer                    |        |
| x4       | tp       | Thread pointer                    |        |
| x5       | t0       | Temporary/alternate link register | Caller |
| x6       | t1       | Temporaries                       | Caller |
| x7       | t2       | Temporaries                       | Caller |
| x8       | s0/fp    | Saved register/frame pointer      | Callee |
| x9       | s1       | Saved register                    | Callee |
| x10      | a0       | Function arguments/return values  | Caller |
| x11      | a1       | Function arguments/return values  | Caller |
| x12      | a2       | Function arguments                | Caller |
| x13      | a3       | Function arguments                | Caller |
| x14      | a4       | Function arguments                | Caller |
| x15      | a5       | Function arguments                | Caller |
| x16      | a6       | Function arguments                | Caller |
| x17      | a7       | Function arguments                | Caller |
| x18      | s2       | Saved registers                   | Callee |
| x19      | s3       | Saved registers                   | Callee |
| x20      | s4       | Saved registers                   | Callee |
| x21      | s5       | Saved registers                   | Callee |
| x22      | s6       | Saved registers                   | Callee |
| x23      | s7       | Saved registers                   | Callee |
| x24      | s8       | Saved registers                   | Callee |
| x25      | s9       | Saved registers                   | Callee |
| x26      | s10      | Saved registers                   | Callee |
| x27      | s11      | Saved registers                   | Callee |
| x28      | t3       | Temporaries                       | Caller |
| x29      | t4       | Temporaries                       | Caller |
| x30      | t5       | Temporaries                       | Caller |
| x31      | t6       | Temporaries                       | Caller |

