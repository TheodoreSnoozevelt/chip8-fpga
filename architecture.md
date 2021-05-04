# System architecture
```graphviz
digraph components {
    graph [rankdir=TB, splines=ortho]
    node [shape=record]
    Memory[label="Memory"]
    I
    PC
    Registers
    Stack[label="Stack pointer"]
    Clock[label="Clock divider (500Hz)"]

    Sound[label="Sound timer"]
    Delay[label="Delay timer"]
    
    Clock2[label="Clock divider (60Hz)"]
    
    
    Memory -> Fetch;

    Fetch 
    ID[label="Decode"];
    Control[label="Control Unit", width=6];
    
    {rank = same; Fetch -> ID; }
    {rank = same; ID -> Control; }
    
    Clock -> Fetch
    PC -> Fetch 
    Fetch -> Memory
    
    Control -> Memory 
    Memory -> Control
    
    Control -> Memory
    Memory -> Control
    Control -> PC
       
    Control -> Stack
    Stack -> Control
    
    Control -> I
    I -> Control
    
    Control -> Registers
    Registers -> Control

    Control -> Sound
    Control -> Delay
    Delay -> Control
    
    Clock2 -> Delay
    Clock2 -> Sound
}
```
## Components
### Registers and memory
| Label         | Name |
| ---------     | -----                               |
| Clock divider | Should run at 500Hz             |
| I             | Address Register (16 bit)           |
| PC            | Instruction Pointer                 | 
| Registers     | 16 8-bit registers                  |
| Stack         | Stack pointer for function return addresses. <br/> The stack is saved in RAM  |
| Delay Timer   | Read-write timer which decrements to 0 at 60 Hz. |
| Sound Timer   | Write-only timer which decrements to 0 at 60 Hz. If the value is nonzero, a sound will play. |
| ALU           | Uses VF as status register. 1x 8-bit operand, 1x 16-bit operand, 1x 16-bit output. |

### Logical
#### Fetch 
Fetches the instruction at memory location PC
#### Decode
Decodes the instruction into operation, registers, addresses
#### Control Unit
Executes the instruction and writes results back to memory and registers

