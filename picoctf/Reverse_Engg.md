# 1. GDB baby step 1

> Can you figure out what is in the `eax` register at the end of the `main` function? Put your answer in the picoCTF flag format: `picoCTF{n}` where `n` is the contents of the `eax` register in the decimal number base. If the answer was `0x11` your flag would be `picoCTF{17}`. Disassemble `this`.

## Solution:
The given file was `debugger0_a` which was an `ELF` file.  
1) First we make the file executable using `chmod +x debugger0_a`  
2) As the name suggests we use `GDB` to debug this program.  
```bash
gdb debugger0_a
(gdb) set disable-randomization on
(gdb) break main
(gdb) run
```
3) - So essentially what we did is we started debugging in gdb,  
   -  we set randomization or `ASLR` to off so that the addresses are stable and do not change everytime we run it because it's a `PIE` file.
   - we set a break point at `main` to help us debug and analyse the code.
   - `run` actually starts the program and will stop at the breakpoint.  

```bash
lallu@VedantLohan:~/picoctf/gdb_baby$ gdb debugger0_a
GNU gdb (Ubuntu 12.1-0ubuntu1~22.04.2) 12.1
Copyright (C) 2022 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from debugger0_a...
(No debugging symbols found in debugger0_a)
(gdb) set dis
disable-randomization  disassembler-options   disconnected-dprintf   displaced-stepping
disassemble-next-line  disassembly-flavor     disconnected-tracing
(gdb) set disable-randomization off
(gdb) break main
Breakpoint 1 at 0x1131
(gdb) run
Starting program: /home/lallu/picoctf/gdb_baby/debugger0_a
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".

Breakpoint 1, 0x00005a9dad300131 in main ()
(gdb) dissemble main
Undefined command: "dissemble".  Try "help".
(gdb)  disassemble main
Dump of assembler code for function main:
   0x00005a9dad300129 <+0>:     endbr64
   0x00005a9dad30012d <+4>:     push   %rbp
   0x00005a9dad30012e <+5>:     mov    %rsp,%rbp
=> 0x00005a9dad300131 <+8>:     mov    %edi,-0x4(%rbp)
   0x00005a9dad300134 <+11>:    mov    %rsi,-0x10(%rbp)
   0x00005a9dad300138 <+15>:    mov    $0x86342,%eax
   0x00005a9dad30013d <+20>:    pop    %rbp
   0x00005a9dad30013e <+21>:    ret
End of assembler dump.
(gdb)
```

4) from the line ` 0x00005a9dad300138 <+15>:    mov    $0x86342,%eax` we understand that `0x86342` is being moved in to the register `eax`. Which means the return value of `main` is `0x86342.

5) converting `0x86342` to dec we get the flag as `picoCTF{549698}`

## Flag:

```
picoCTF{549698}
```

## Concepts learnt:
- ELF  
   It stands for "Executable and Linkable", it is basically `.exe` for linux

- PIE  
   It stands for "Position Independent Executable", it is a kind of a program that can be loaded at any memory address in the RAM

- ASLR  
   Address Space Layout Randomization. It randomizes where code, stack, heap, etc. are loaded in memory each time you run a program.
- offset  
   It basically tells you how far is something from a known starting point

- eax registor  
   It is a general purpose registor in X86 arch, and a registor is basically a memory unit in the cpu

- gdb  
   gdb is a debugging tool to debug programs

- mov commands
   copies data from source to destination, `mov destination, source`



## Notes:

- One mistake was that I had forgotten to make the program executable.


## Resources:

[hex to dec](https://www.rapidtables.com/convert/number/hex-to-decimal.html?x=86342)
[GDB](https://www.youtube.com/watch?v=Dq8l1_-QgAc)

***

# 2. ARMssembly 1

> For what argument does this program print `win` with variables 68, 2 and 3? File: chall_1.S Flag format: picoCTF{XXXXXXXX} -> (hex, lowercase, no 0x, and 32 bits. ex. 5614267 would be picoCTF{0055aabb})

## Solution:
In this challenge we had been given an ARM assembly file, we had to understand the code to solve the challenge

```
lallu@VedantLohan:~/picoctf/arm$ cat chall_1.S
        .arch armv8-a
        .file   "chall_1.c"
        .text
        .align  2
        .global func
        .type   func, %function
func:
        sub     sp, sp, #32
        str     w0, [sp, 12]
        mov     w0, 68
        str     w0, [sp, 16]
        mov     w0, 2
        str     w0, [sp, 20]
        mov     w0, 3
        str     w0, [sp, 24]
        ldr     w0, [sp, 20]
        ldr     w1, [sp, 16]
        lsl     w0, w1, w0
        str     w0, [sp, 28]
        ldr     w1, [sp, 28]
        ldr     w0, [sp, 24]
        sdiv    w0, w1, w0
        str     w0, [sp, 28]
        ldr     w1, [sp, 28]
        ldr     w0, [sp, 12]
        sub     w0, w1, w0
        str     w0, [sp, 28]
        ldr     w0, [sp, 28]
        add     sp, sp, 32
        ret
        .size   func, .-func
        .section        .rodata
        .align  3
.LC0:
        .string "You win!"
        .align  3
.LC1:
        .string "You Lose :("
        .text
        .align  2
        .global main
        .type   main, %function
main:
        stp     x29, x30, [sp, -48]!
        add     x29, sp, 0
        str     w0, [x29, 28]
        str     x1, [x29, 16]
        ldr     x0, [x29, 16]
        add     x0, x0, 8
        ldr     x0, [x0]
        bl      atoi
        str     w0, [x29, 44]
        ldr     w0, [x29, 44]
        bl      func
        cmp     w0, 0
        bne     .L4
        adrp    x0, .LC0
        add     x0, x0, :lo12:.LC0
        bl      puts
        b       .L6
.L4:
        adrp    x0, .LC1
        add     x0, x0, :lo12:.LC1
        bl      puts
.L6:
        nop
        ldp     x29, x30, [sp], 48
        ret
        .size   main, .-main
        .ident  "GCC: (Ubuntu/Linaro 7.5.0-3ubuntu1~18.04) 7.5.0"
        .section        .note.GNU-stack,"",@progbits
```
The function `func` performs some operations on the argument `main` prints `"You win!"` if it returns `0`;  
The working of `func`  
```
func:
    sub     sp, sp, #32           // make space on stack
    str     w0, [sp, 12]          // save input 'a' at sp+12

    mov     w0, 68
    str     w0, [sp, 16]          // store 68
    mov     w0, 2
    str     w0, [sp, 20]          // store 2
    mov     w0, 3
    str     w0, [sp, 24]          // store 3

    ldr     w0, [sp, 20]          // load 2
    ldr     w1, [sp, 16]          // load 68
    lsl     w0, w1, w0            // w0 = 68 << 2  (left shift 68 by 2 bits = 68 * 4 = 272)
    str     w0, [sp, 28]          // store 272

    ldr     w1, [sp, 28]          // w1 = 272
    ldr     w0, [sp, 24]          // w0 = 3
    sdiv    w0, w1, w0            // w0 = 272 / 3 = 90 (integer division)
    str     w0, [sp, 28]          // store 90

    ldr     w1, [sp, 28]          // w1 = 90
    ldr     w0, [sp, 12]          // w0 = input
    sub     w0, w1, w0            // w0 = 90 - input
    str     w0, [sp, 28]          // store result
    ldr     w0, [sp, 28]          // return value = 90 - input

    add     sp, sp, 32
    ret

```
`func(a) = 90 - a`
`a=90`
`a = 90` in hexadecimal is `0x0000005a`
## Flag:

```
picoCTF{0000005a}
```

## Concepts learnt:

- sp
   It stand for the stack pointer and it’s a special purpose register that points to the top of the stack in memory.
- `sub` is for subtract, `str` is for store, `ldr` is load register, `lsl` means logical shift(a << b), `sdiv` is signed division, `cmp` is for compare and `bne` is for branch if not equal 


## Resources:

- [chatGPT](https://chatgpt.com/)


***

# 3. Vault door 3

> This vault uses for-loops and byte arrays. The source code for this vault is here: VaultDoor3.java

## Solution:
In this challenge, we have been given a Java program which is a vault which opens when the correct password string is entered. We need to figure out that string to open the vault.

```java
import java.util.*;

class VaultDoor3 {
    public static void main(String args[]) {
        VaultDoor3 vaultDoor = new VaultDoor3();
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter vault password: ");
        String userInput = scanner.next();
	String input = userInput.substring("picoCTF{".length(),userInput.length()-1);
	if (vaultDoor.checkPassword(input)) {
	    System.out.println("Access granted.");
	} else {
	    System.out.println("Access denied!");
        }
    }

    // Our security monitoring team has noticed some intrusions on some of the
    // less secure doors. Dr. Evil has asked me specifically to build a stronger
    // vault door to protect his Doomsday plans. I just know this door will
    // keep all of those nosy agents out of our business. Mwa ha!
    //
    // -Minion #2671
    public boolean checkPassword(String password) {
        if (password.length() != 32) {
            return false;
        }
        char[] buffer = new char[32];
        int i;
        for (i=0; i<8; i++) {
            buffer[i] = password.charAt(i);
        }
        for (; i<16; i++) {
            buffer[i] = password.charAt(23-i);
        }
        for (; i<32; i+=2) {
            buffer[i] = password.charAt(46-i);
        }
        for (i=31; i>=17; i-=2) {
            buffer[i] = password.charAt(i);
        }
        String s = new String(buffer);
        return s.equals("jU5t_a_sna_3lpm12g94c_u_4_m7ra41");
    }
}
```
What this code does is that it takes in the string, removes `picoCTF` from it and grants you the access to the vault if the function `checkpassowrd` returns true.  
Now if we look into the checkpassword function we understand that our password should be 32 chars long. What this function is doing is that it performs certain manipulations on the characters of the strings and shuffles it up. It then saves it into `buffer[i]`, if `buffer[i]` matches `"jU5t_a_sna_3lpm12g94c_u_4_m7ra41"` then it would grant us the access to the vault.  
This means we just have to reverse the logic of this function on the string `"jU5t_a_sna_3lpm12g94c_u_4_m7ra41"` to get the password !

I wrote a C program which takes in the buffer string and performs all the operations in reverse to get back to the password.
To reverse the logic, the last loop is ran in reverse first because it was the last operation that was performed and so on.

```C
#include <stdio.h>
int main() {
    char buffer[33]="jU5t_a_sna_3lpm12g94c_u_4_m7ra41";
    char password[33];
    int i;
    for (i=17; i<=31; i+=2) {
        password[i] = buffer[i];
    }
    for (i=16; i<=30; i+=2) {
        password[i] = buffer[46-i];
    }
    for (i=8; i<=15; i++) {
        password[i] = buffer[23-i];
    }
    for (i=0; i<8; i++) {
        password[i] = buffer[i];
    }
   printf("%s",password);
    return 0;
}
```
This outputs -  
```
jU5t_a_s1mpl3_an4gr4m_4_u_c79a21
```
## Flag:

```
picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_c79a21}
```

## Concepts learnt:

- Learned a bit of Java


## Notes:

- I was making mistakes while reversing the logic, I tried to figure out and track each index which was very lenghty approach.

## Resources:
- my roommate who knows java


***
