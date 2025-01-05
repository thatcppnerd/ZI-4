# Adders


## Half Adders
Half Adders are 1-bit binary adders, taking in two inputs($I_0, I_1$), and giving two outputs($R$ (Result) and $C$ (Carry)).


$R = I_0 \oplus I_1$  
$C = I_0 * I_1$

Without an XOR, $R$ can be expressed like $R = (I_0 * I_1 \prime) + (I_0 \prime * I_1)$.

## Full Adders
Full Adders are typically made by chaining many Half Adders together ($R$ connected to output pin, $C$ connected to next adder) to form multi-bit adders.



## Signed 4-bit Adder

0 - positive  
1 - negative

3 bits regular arithmetic  
1 bit sign

0 is always positive, encoded as $0000_{2}$.

truth table  
| $I_0$ (3) | $I_0$ (2:0) | $I_1$ (3) | $I_1$ (2:0) | $O_0$ (3) | O_0 (2:0) | Carry |
| --------- | ----------- | --------- | ----------- | --------  | --------- | ----- |
| 0 | 000 | 0 | 000 | 0 | 000 | 0 |
| 0 | 000 | 0 |
| 1 | 111 | 1 | 111 | 0 | 111 | 1 |