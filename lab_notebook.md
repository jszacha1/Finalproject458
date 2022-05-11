# Buffer Overflow Attack Lab
This lab took my about 5 hours from start to do the first 3 tasks. However, I am not the best person to judge the time off of because I am currently suffering from a concussion and am not thinking very clearly. I believe I could have done it in near half the time if I was operating normally. Simply setting the VM up took me almost 2 hours because I had trouble looking for icons on the application. An extra hour trying to find what I was doing wrong, even though things worked but I assumed they would look different was also used. 

## Task 1: Getting Familiar with Shellcode
### Section 3.4
When executing shell code the terminal seed@VM user. I am able to run normal commands like ls, echo, and sleep in shellcode just like in the standard user space. Noticable shellcode doesn't support the backspace or delete when trying to type in the command line.

![Shell code execution](https://user-images.githubusercontent.com/46972037/167742282-61118f7e-211d-4704-908a-d255a8205bc0.png)

## Task 2: Understanding the Vulnerable Program
### Section 4
This was just compiling the program which worked, so nothing particularly interesting happened.

## Task 3: Launching Attack on 32-bit Program (Level 1)
### Section 5.1
![Investigation](https://user-images.githubusercontent.com/46972037/167753308-ed177966-7cba-4c63-a897-2df3d6cf1f69.png)
### Section 5.2
For the shellcode portion of the exploit.py function, I simply copied the 32bit code from the call_shellcode.c file. The start in content was 517 - the length of the shellcode. This also worked with values around 517, but 517 was chosen as it appeared in stack.c. The length of the shell code was subtracted because content only needed to be the length of the shell code from start to finish. I originally thought ret was just the address of ebp and the offset was the address of ebp - &buffer, since that would be the offset of the return address from the start of the function. However, this didn't work. After a lot of trial and error 4, for a single frame was added to the offset to make it work and that offset + another frame of 4 was added to the return address. For values of 112, and 0xffffcb98 + 116 respectively.
![Successful Root Shell Attack](https://user-images.githubusercontent.com/46972037/167753141-dfa8a267-8e26-43d2-b8d4-02698e18474d.png)




