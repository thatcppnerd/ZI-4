# Adders


## Half Adders
Half Adders are 1-bit binary adders, taking in two inputs($I_0, I_1$), and giving two outputs($R$ (Result) and $C$ (Carry)).


$R = I_0 \oplus I_1$  
$C = I_0 * I_1$

Without an XOR, $R$ can be expressed like $R = (I_0 * I_1 \prime) + (I_0 \prime * I_1)$.

## Full Adders
Full Adders are typically made by chaining many Half Adders together ($R$ connected to output pin, $C$ connected to next adder) to form multi-bit adders.



