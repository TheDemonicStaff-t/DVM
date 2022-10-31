# FILE FORMAT

'.DVM'
4b version          (f32)
2b entrie point idx
2b include count    (u16)
2b[] include index  (index into string pool(u16))
2b num 32 count     (u16)
num_32[] 32 bit numbers (tag, (i32, u32, or f32)) tag = (0=i, 1=u, 2=f)
    tag = 2 bits, (i, u, f) 1 possible tag unused
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

# OPCODES
0x:name : agrc, type : stack state : Desc
00:lp_4 : 1, index   : null -> 4b  : pull 4 bytes from the num32 pool at index
01:lp_8 : 1, index   : null -> 8b  : pull 8 bytes from the num64 pool at index
02:lp_4x: 2, index   : null -> 4b  : pull 4 bytes from the num32 pool at index extended
03:lp_8x: 2, index   : null -> 8b  : pull 8 bytes from the num64 pool at index extended