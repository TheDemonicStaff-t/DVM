# FILE FORMAT

'.DVM'
4b version          (f32)
2b entrie point idx

2b include count    (u16)
2b[] include index  (index into string pool(u16))

2b num 64 count 
num_64[] 64 bit numbers (tag, (i64, u64, or f64))  tag = (0=i, 1=u, 2=f)
    tag = 2 bits, (i, u, f) 1 possible tag unused

2b Utf8 count
Utf8[] string (size, char[])

2b Array count
Array[] Arrays (data type, var count, type[])
    type[] = type A[var count]

2b Struct def count
Struct_def[] Struct def (var_count (u16), var[])
    var[] = every 4 bits is a variable type (u8, u16, u32 u64, i8, i16, i32, i64, f32, f64, string, Array) 12/16 possible variable types

Struct count
Structs[] Structures (struct_def, data[var_count] = (var[var_count]))

Function count
Functions[] function (locals_count, var[var_count], ret_type, 2b max stack, code size, code[])

# BYTECODE INFO
opcode is always 1 byte
opcode can have up to 4 bytes in operands

pushing can source from args or pool

storing is always from stack to memory

loading is always from memory to stack

# OPCODES
::pushes::
00: push8   : 1b, data  : nul -> 32
01: push16  : 2b, data  : nul -> 32
02: push32  : 4b, data  : nul -> 32
03: push64c : 1b, index : nul -> 64
04: push64cx: 2b, index : nul -> 64

05: pushUr  : 1b, index : nul -> 16
06: pushUrx : 2b, index : nul -> 16

07: pushSr  : 1b, index : nul -> 16
08: pushSrx : 2b, index : nul -> 16

09: pop1    : nul       : 32 -> nul
0a: pop2    : nul       : 64 -> nul
0b: popr    : nul       : 16 -> nul

::arithmetic::
0c: iadd32  : nul       : i32 i32 -> i32
0d: uadd32  : nul       : u32 u32 -> u32
0e: fadd32  : nul       : f32 f32 -> f32
0f: iadd64  : nul       : i64 i64 -> i64
10: uadd64  : nul       : u64 u64 -> u64
11: fadd64  : nul       : f64 f64 -> f64

12: isub32  : nul      : i32 i32 -> i32
13: usub32  : nul      : u32 u32 -> u32
14: fsub32  : nul      : f32 f32 -> f32
15: isub64  : nul      : i64 i64 -> i64
16: usub64  : nul      : u64 u64 -> u64
17: fsub64  : nul      : f64 f64 -> f64

18: imul32  : nul      : i32 i32 -> i32
19: umul32  : nul      : u32 u32 -> u32
1a: fmul32  : nul      : f32 f32 -> f32
1b: imul64  : nul      : i64 i64 -> i64
1c: umul64  : nul      : u64 u64 -> u64
1d: fmul64  : nul      : f64 f64 -> f64

1e: idiv32  : nul      : i32 i32 -> i32
1f: udiv32  : nul      : u32 u32 -> u32
20: fdiv32  : nul      : f32 f32 -> f32
21: idiv64  : nul      : i64 i64 -> i64
22: udiv64  : nul      : u64 u64 -> u64
23: fdiv64  : nul      : f64 f64 -> f64

::stores::
24: str32   : 1b, index: 32 -> nul
25: str32x  : 2b, index: 32 -> nul

26: str64   : 1b, index: 64 -> nul
27: str64x  : 2b, index: 64 -> nul

28: strUr   : 1b, index: 16 -> nul (mov data of ref as array)
29: strUrx  : 2b, index: 16 -> nul (mov data of ref as array)

2a: strSr   : 1b, index: 16 -> nul (mov data of ref as array)
2b: strSrx  : 2b, index: 16 -> nul (mov data of ref as array)

::invokes::
2c: invN    : 1b, index: * -> nul 
2d: invNx   : 2b, index: * -> nul 

2e: inv32   : 1b, index: * -> 32 
2f: inv32x  : 2b, index: * -> 32 

2e: inv64   : 1b, index: * -> 64 
2f: inv64x  : 2b, index: * -> 64

30: invU    : 1b, index: * -> 16 (address in memory)
31: invUx   : 2b, index: * -> 16 (address in memory)

32: invS    : 1b, index: * -> 16 (address in memory)
33: invSx   : 2b, index: * -> 16 (address in memory)

::loads::