## Concept 
rax = y

XOR rax , rax : This clears the rax register , therefore rax = 0

rdi =  x

To check if x is even : rdi AND 1 

- If LSB is 1 , result will be 1 indicating its odd
- If LSB is 0 , result will be 0 indicating its even

OR rax, rdi  : In this since our rax = 0 , the or instruction basically just acts a mov instruction copying values of rdi into rax

XOR rax, 1 : This will flip bit of rax 

- If rax was 1 it will be 0 , if it was 0 it will be 1
- Therefore if rax will be 1 if it was even , it will be 0 if it was odd

```sh

hacker@assembly-crash-course~level11:~$ /challenge/run

Welcome to ASMLevel11
==================================================

To interact with any level you will send raw bytes over stdin to this program.
To efficiently solve these problems, first run it to see the challenge instructions.
Then craft, assemble, and pipe your bytes to this program.

For instance, if you write your assembly code in the file asm.S, you can assemble that to an object file:
  as -o asm.o asm.S

Note that if you want to use Intel syntax for x86 (which, of course, you do), you'll need to add the following to the start of asm.S:
  .intel_syntax noprefix

Then, you can copy the .text section (your code) to the file asm.bin:
  objcopy -O binary --only-section=.text asm.o asm.bin

And finally, send that to the challenge:
  cat ./asm.bin | /challenge/run

You can even run this as one command:
  as -o asm.o asm.S && objcopy -O binary --only-section=.text ./asm.o ./asm.bin && cat ./asm.bin | /challenge/run

In this level you will be working with registers. You will be asked to modify
or read from registers.

We will now set some values in memory dynamically before each run. On each run
the values will change. This means you will need to do some type of formulaic
operation with registers. We will tell you which registers are set beforehand
and where you should put the result. In most cases, its rax.

In this level you will be working with bit logic and operations. This will involve heavy use of
directly interacting with bits stored in a register or memory location. You will also likely
need to make use of the logic instructions in x86: and, or, not, xor.



Using only the following instructions:
  and, or, xor

Implement the following logic:
  if x is even then
    y = 1
  else
    y = 0

where:
  x = rdi
  y = rax

We will now set the following in preparation for your code:
  rdi = 0x2772862e

Please give me your assembly in bytes (up to 0x1000 bytes):

```

## Solution 

```python
import pwn
pwn.context.update(arch="amd64")
code = pwn.asm("""

xor rax, rax
and rdi, 1
or rax, rdi
xor rax, 1


""")
process = pwn.process("/challenge/run")
process.write(code)
print(process.readall())

```
