# AGAL bytecode format {#agal-bytecode-format}

AGAL bytecode must use Endian.LITTLE_ENDIAN format.

Bytecode Header

AGAL bytecode must begin with a 7-byte header:

A0 01000000 A1 00 -- for a vertex program A0 01000000 A1 01 -- for a fragment program

| **Offset (bytes)** | **Size (bytes)** | **Name** | **Description** |
| --- | --- | --- | --- |
| 0 | 1 | magic | must be 0xa0 |
| 1 | 4 | version | must be 1 |
| 5 | 1 | shader type ID | must be 0xa1 |
| 6 | 1 | shader type | 0 for a vertex program; 1 for a fragment program |

Tokens

The header is immediately followed by any number of tokens. Every token is 192 bits (24 bytes) in size and always has the format:

[opcode][destination][source1][source2 or sampler]

Not every opcode uses all of these fields. Unused fields must be set to 0.

Operation codes

The [opcode] field is 32 bits in size and can take one of these values:

| **Name** | **Opcode** | **Operation** | **Description** |
| --- | --- | --- | --- |
| mov | 0x00 | move | move data from source1 to destination, component-wise |
| add | 0x01 | add | destination = source1 + source2, component-wise |
| sub | 0x02 | subtract | destination = source1 - source2, component-wise |
| mul | 0x03 | multiply | destination = source1 * source2, component-wise |
| div | 0x04 | divide | destination = source1 / source2, component-wise |
| rcp | 0x05 | reciprocal | destination = 1/source1, component-wise |

| **Name** | **Opcode** | **Operation** | **Description** |
| --- | --- | --- | --- |
| min | 0x06 | minimum | destination = minimum(source1,source2), component-wise |
| max | 0x07 | maximum | destination = maximum(source1,source2), component-wise |
| frc | 0x08 | fractional | destination = source1 - (float)floor(source1), component-wise |
| sqt | 0x09 | square root | destination = sqrt(source1), component-wise |
| rsq | 0x0a | reciprocal root | destination = 1/sqrt(source1), component-wise |
| pow | 0x0b | power | destination = pow(source1,source2), component-wise |
| log | 0x0c | logarithm | destination = log_2(source1), component-wise |
| exp | 0x0d | exponential | destination = 2^source1, component-wise |
| nrm | 0x0e | normalize | destination = normalize(source1), component-wise (produces only a 3 component result, destination must be masked to .xyz or less) |
| sin | 0x0f | sine | destination = sin(source1), component-wise |
| cos | 0x10 | cosine | destination = cos(source1), component-wise |
| crs | 0x11 | cross product | destination.x = source1.y * source2.z - source1.z * source2.y destination.y = source1.z * source2.x - source1.x * source2.z destination.z = source1.x * source2.y - source1.y * source2.x |
| dp3 | 0x12 | dot product | destination = source1.x*source2.x + source1.y*source2.y + source1.z*source2.z |
| dp4 | 0x13 | dot product | destination = source1.x*source2.x + source1.y*source2.y + source1.z*source2.z + source1.w*source2.w |
| abs | 0x14 | absolute | destination = abs(source1), component-wise |
| neg | 0x15 | negate | destination = -source1, component-wise |
| sat | 0x16 | saturate | destination = maximum(minimum(source1,1),0), component-wise |
| m33 | 0x17 | multiply matrix 3x3 | destination.x = (source1.x * source2[0].x) + (source1.y * source2[0].y) |
| m44 | 0x18 | multiply matrix 4x4 | destination.x = (source1.x * source2[0].x) + (source1.y * source2[0].y) |

| **Name** | **Opcode** | **Operation** | **Description** |
| --- | --- | --- | --- |
| m34 | 0x19 | multiply matrix 3x4 | destination.x = (source1.x * source2[0].x) + (source1.y * source2[0].y) |
| kil | 0x27 | kill/discard (fragment shader only) | If single scalar source component is less than zero, fragment is discarded and not drawn to the frame buffer. (Destination register must be set to all 0) |
| tex | 0x28 | texture sample (fragment shader only) | destination equals load from texture source2 at coordinates source1\. In this case, source2 must be in sampler format. |
| sge | 0x29 | set-if-greater-equal | destination = source1 &gt;= source2 ? 1 : 0, component-wise |
| slt | 0x2a | set-if-less-than | destination = source1 &lt; source2 ? 1 : 0, component-wise |
| seq | 0x2c | set-if-equal | destination = source1 == source2 ? 1 : 0, component-wise |
| sne | 0x2d | set-if-not-equal | destination = source1 != source2 ? 1 : 0, component-wise |

In AGAL2, the following opcodes have been introduced:

| **Name** | **Opcode** | **Operation** | **Description** |
| --- | --- | --- | --- |
| ddx | 0x1a | partial derivative in X | Load partial derivative in X of source1 into destination. |
| ddy | 0x1b | partial derivative in Y | Load partial derivative in Y of source1 into destination. |
| ife | 0x1c | if equal to | Jump if source1 is equal to source2. |
| ine | 0x1d | if not equal to | Jump if source1 is not equal to source2. |
| ifg | 0x1e | if greater than | Jump if source1 is greater than or equal to source2. |
| ifl | 0x1f | if less than | Jump if source1 is less than source2. |
| els | 0x20 | else | Else block |
| eif | 0x21 | Endif | Close if or else block. |

Destination field format

The [destination] field is 32 bits in size:

31.............................0

----TTTT----MMMMNNNNNNNNNNNNNNNN

T = Register type (4 bits) M = Write mask (4 bits)

N = Register number (16 bits)

- = undefined, must be 0

Source field format

The [source] field is 64 bits in size:

63.............................................................0

D-------------QQ----IIII----TTTTSSSSSSSSOOOOOOOONNNNNNNNNNNNNNNN

D = Direct=0/Indirect=1 for direct Q and I are ignored, 1bit Q = Index register component select (2 bits)

I = Index register type (4 bits) T = Register type (4 bits)

S = Swizzle (8 bits, 2 bits per component) O = Indirect offset (8 bits)

N = Register number (16 bits)

- = undefined, must be 0

Sampler field format

The second source field for the tex opcode must be in [sampler] format, which is 64 bits in size:

63.............................................................0

FFFFMMMMWWWWSSSSDDDD--------TTTT--------BBBBBBBBNNNNNNNNNNNNNNNN

N = Sampler register number (16 bits)

B = Texture level-of-detail (LOD) bias, signed integer, scale by 8\. The floating point value used is b/8.0 (8 bits) T = Register type, must be 5, Sampler (4 bits)

F = Filter (0=nearest,1=linear) (4 bits)

M = Mipmap (0=disable,1=nearest, 2=linear) W = Wrapping (0=clamp,1=repeat)

S = Special flag bits (must be 0) D = Dimension (0=2D, 1=Cube)

Program Registers

The number of registers used depend upon the Context3D profile used. The number of registers along with their usage are defined in the following table:

| **Name AGAL2 AGAL3** | **Value** | **AGAL** |
| --- | --- | --- |
| Number per fragment program |  | Number per fragment program | Number per vertex program |
| Context 3D Profiles Support |  | Below Standard |
| SWF |  | Below 25 |
| Attribute NA | 0 | NA | 8 |

| **Name AGAL2 AGAL3** | **Value** | **AGAL** |
| --- | --- | --- |
| Constant 64 | 1 | 28 | 128 |
| Temporary 26 | 2 | 8 | 8 |
| Output 1 | 3 | 1 | 1 |

| **Name AGAL2 AGAL3** | **Value** | **AGAL** |
| --- | --- | --- |
| Varying 10 | 4 | 8 | 8 |

| **Name AGAL2 AGAL3** | **Value** | **AGAL** |
| --- | --- | --- |
| Sampler 16 | 5 | 8 | NA |
| Fragment register | 6 | NA | NA |
| Tokens 1024 |  | 200 |

The latest AGAL Mini Assembler can be found [here](https://github.com/adobe-flash/graphicscorelib/tree/master/src/com/adobe/utils/v3).