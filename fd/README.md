### Mommy! what is a file descriptor in Linux?
```
File Descriptors (FD) are non-negative integers that the kernel uses to 
identify the files accessed by a process. When we open an existing file or 
create a new one, the parent process forks a process, the child process 
inherits the file descriptors of the parent and kernel returns a file 
descriptor to the calling code.
```
```
─────┬─────────────────────────────────────────────────────────────────────────
     │ File: fd.c
─────┼─────────────────────────────────────────────────────────────────────────
   1 │ #include <stdio.h>
   2 │ #include <stdlib.h>
   3 │ #include <string.h>
   4 │ char buf[32];
   5 │ int main(int argc, char* argv[], char* envp[]){
   6 │     if(argc<2){
   7 │         printf("pass argv[1] a number\n");
   8 │         return 0;
   9 │     }
  10 │     int fd = atoi( argv[1] ) - 0x1234;
  11 │     int len = 0;
  12 │     len = read(fd, buf, 32);
  13 │     if(!strcmp("LETMEWIN\n", buf)){
  14 │         printf("good job :)\n");
  15 │         setregid(getegid(), getegid());
  16 │         system("/bin/cat flag");
  17 │         exit(0);
  18 │     }
  19 │     printf("learn about Linux file IO\n");
  20 │     return 0;
  21 │
  22 │ }
  23 │
─────┴─────────────────────────────────────────────────────────────────────────
```
```
Program handles 1 argument, which is used as fd parameter for read(3), also the 
hexadecimal 0x1234 (4660 in dec.) substracted from it. To get flag we need to 
pass LETMEWIN into read(3), passing fd 0 will open STDIN, so we need to pass
0x1234 as decimal input.
```
```
❯ ./fd 0
learn about Linux file IO
❯ ./fd 4660
LETMEWIN
good job :)
flag{this_is_a_dummy_flag}
```

