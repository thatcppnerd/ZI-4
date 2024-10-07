# ALU

## Inputs
| Name       | Bits   | Description                                                                                    |
| ---------- | :----- | ---------------------------------------------------------------------------------------------- |
| OPCODE     | 2 : 0  | A 3-bit value representing the desired operation. (See [Opcode Table](#opcode-value-table))    |
| ALT_OPCODE | 3      | If this is set HIGH, then ALU will use alternate opcodes (signed arithmetic, bit-shifting ops) |
| INPUT 0    | 7 : 4  | 4-bit input                                                                                    |
| INPUT 1    | 11 : 8 | 4-bit input                                                                                    |

## Outputs
| Name         | Bits  | Description                                                              |
| ------------ | ----- | ------------------------------------------------------------------------ |
| OUTPUT 0     | 3 : 0 | 4-bit output                                                             |
| OUTPUT 1     | 7 : 4 | 4-bit output                                                             |
| CARRY        | 8     | Is set HIGH when output can't fit into $O_0$ and spills over into $O_1$. |
| SIGN         | 9     | Is set HIGH when output is negative.                                     |
| OVERFLOW     | 10    | Is set HIGH when integer overflow happens, negatively or positively.     |
| ZERO         | 11    | Is set HIGH when all relevant outputs are zero.                          |
| EQUAL        | 12    | Is set HIGH when $I_0 = I_1$.                                            |
| LESS/GREATER | 13    | Is set HIGH when $I_0 \gt I_1$, and set LOW when $I_0 \le I_1$.          |
| PARITY       | 14    | Is HIGH when output is **odd**, LOW when output is **even**.             |

## Opcode Value Tables
### Default (ALT_OPCODE = 0)
| Opcode | Operation |
| ------ | --------- |
| 0x0    | ADD       |
| 0x1    | SUB       |
| 0x2    | MUL       |
| 0x3    | DIV       |
| 0x4    | CMP       |
| 0x5    | AND       |
| 0x6    | OR        |
| 0x7    | XOR       |

### Alternate (ALT_OPCODE = 1)
| Opcode | Operation |
| ------ | --------- |
| 0x8    | IADD      |
| 0x9    | ISUB      |
| 0xA    | IMUL      |
| 0xB    | IDIV      |
| 0xC    | SHL       |
| 0xD    | SHR       |
| 0xE    | ROL       |
| 0xF    | ROR       |


# Operations
* $I_x - Input$
* $O_x - Output$

## ADD (Unsigned Addition With Carry)
$O_0 = (I_0 + I_1) \And 1111_2$  
$O_1 = (I_0 + I_1) >> 4$  

### Flags
$Carry = (I_0 + I_1) \ge 10_{16}$  
$Parity = O_0 \And 2_{16}$  

## SUB (Unsigned Subtraction With Carry)
$O_0 = (I_0 - I_1) \And 1111_2$  
$O_1 = (I_0 - I_1) >> 4$

### Flags
$Carry = O_0 \ge 10_{16}$  
$Parity = O_0 \And 2_{16}$

## MUL (Unsigned Multiply)
$I_0 = Multiplicand$  
$I_1 = Multiplier$  
$O_0 = (I_0 * I_1) \And 1111_2$  
$O_1 = (I_0 * I_1) >> 4$ 

### Flags
$Carry = O_0 \ge 10_{16}$  
$Parity = O_0 \And 2_{16}$

## DIV
$I_0 = Dividend$  
$I_1 = Divisor$  
$O_0 = Quotient = \Large\frac{I_0}{I_1}$  
$O_1 = Remainder = I_0 \bmod I_1$ 

## CMP (Compare)
$Less/Greater =$

## AND
$O_0 = I_0 \And I_1$  
$O_1 = 0000_2$

## OR
$O_0 = I_0 \And I_1$  
$O_1 = 0000_2$

## XOR
$O_0 = I_0 \oplus I_1$  
$O_1 = 0000_2$


# Internals

## What happens to the opcode?
When an input is given in the **OPCODE** selector, it is sent through a de-multiplexer and outputs HIGH on the corresponding pin.  The signal is then diverted to either the normal or alternate **operators**, according to the state of the **ALT_OPCODE** pin.

## What are the operators?
The operators contain the actual arithmetic circuitry for the ALU.  There are two operators, the normal operator and the alternate operator.  They are