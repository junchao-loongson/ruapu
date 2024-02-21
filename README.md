# ruapu
Detect cpu ISA features with single-file

<table>
<tr><td>CPU</td><td>&#9989; x86, x86-64<br/>&#9989; arm, aarch64</td><td rowspan=3>
  
```c
#define RUAPU_IMPLEMENTATION
#include "ruapu.h"

int main()
{
    // initialize ruapu once
    ruapu_init();

    // now, tell me if this cpu has avx2
    int has_avx2 = ruapu_supports("avx2");

    return 0;
}
```

</td></tr>
<tr><td>OS</td><td>&#9989; Windows<br/>&#9989; Linux<br/>&#9989; macOS<br/>&#9989; Android<br/>&#9989; iOS</td></tr>
<tr><td>Compiler</td><td>&#9989; GCC<br/>&#9989; Clang<br/>&#9989; MSVC<br/>&#9989; MinGW</td></tr>
</table>

## Let's ruapu

<table>

<tr><td>

Compile ruapu test program

```shell
# GCC / MinGW
gcc main.c -o ruapu
```
```shell
# Clang
clang main.c -o ruapu
```
```shell
# MSVC
cl.exe /Fe: ruapu.exe main.c
```
</td>
<td>

Run ruapu in command line

```shell
./ruapu 
mmx = 1
sse = 1
sse2 = 1
sse3 = 1
ssse3 = 1
sse41 = 1
sse42 = 1
sse4a = 1
xop = 0
... more lines omitted ...
```

</td></tr>
</table>

## Features

* Detect **CPU ISA with single-file**&emsp;&emsp;&emsp;
_`x86-sse2`, `x86-avx`, `x86-avx512f`, `arm-neon`, etc._
* Detect **vendor extended ISA**&emsp;&emsp;&emsp;&emsp;
_apple `amx`, risc-v vendor ISA, etc._
* Detect **richer ISA on Windows ARM**&emsp;&emsp;
_`IsProcessorFeaturePresent()` returns little ISA information_
* Detect **`x86-avx512` on macOS correctly**&emsp;
_macOS hides it in `cpuid`_
* Detect **new CPU's ISA on old systems**&emsp;
_they are usually not exposed in `auxv` or `MISA`_
* Detect **CPU hidden ISA**&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
_`x86-fma4` on zen1, ISA in hypervisor, etc._

## Techniques inside ruapu
ruapu is implemented in C language to ensure the widest possible portability.

ruapu determines whether the CPU supports certain instruction sets by trying to execute instructions and detecting whether an `Illegal Instruction` exception occurs. ruapu does not rely on the cpuid instructions and registers related to the CPU architecture, nor does it rely on the `MISA` information and system calls of the operating system. This can help us get more detailed CPU ISA information.

