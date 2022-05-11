# Buffer Overflow Attack Lab
This lab took my about 5 hours from start to do the first 3 tasks. However, I am not the best person to judge the time off of because I am currently suffering from a concussion and am not thinking very clearly. I believe I could have done it in near half the time if I was operating normally. Simply setting the VM up took me almost 2 hours because I had trouble looking for icons on the application. I also wasted an hour trying to find out what I was doing wrong when in reality the everything was working fine, I just assumed it would look different.

## Task 1: Getting Familiar with Shellcode
### Section 3.4
When executing shell code the terminal seed@VM user. I am able to run normal commands like ls, echo, and sleep in shellcode just like in the standard user space. Noticable shellcode doesn't support the backspace or delete when trying to type in the command line.

![Shell code execution](https://user-images.githubusercontent.com/46972037/167742282-61118f7e-211d-4704-908a-d255a8205bc0.png)

## Task 2: Understanding the Vulnerable Program
### Section 4
This was just compiling the program which worked, so nothing particularly interesting happened. stack.c reads in a file called badfile then calls Bof on it which is supposed to copy the contents of badfile to a string then the program returns successfully. The program's vulneribility is in the fact that the buffersize when reading in the file is different from the buffer size in Bof. 

## Task 3: Launching Attack on 32-bit Program (Level 1)
### Section 5.1
![Investigation](https://user-images.githubusercontent.com/46972037/167753308-ed177966-7cba-4c63-a897-2df3d6cf1f69.png)
### Section 5.2
For the shellcode portion of the exploit.py function, I simply copied the 32bit code from the call_shellcode.c file. The start in content was 517 - the length of the shellcode. This also worked with values around 517, but 517 was chosen as it appeared in stack.c. The length of the shell code was subtracted because content only needed to be the length of the shell code from start to finish. I originally thought ret was just the address of ebp and the offset was the address of ebp - &buffer, since that would be the offset of the return address from the start of the function which was 108. However, this didn't work. After some trial and error 4, for a single frame was added to the offset to make it work and that offset + another frame of 4 was added to the return address. For values of 112, and 0xffffcb98 + 116 respectively.
![Successful Root Shell Attack](https://user-images.githubusercontent.com/46972037/167753141-dfa8a267-8e26-43d2-b8d4-02698e18474d.png)

## Task 4: Launching Attack without Knowing Buffer Size
### Section 6
I don't know if this is the correct method but, to figure out the buffer size, you need to figure out the address of buffer. To find the address of buffer one can simply look at the top of the stack or the address of ESP for the stack pointer. If the address points to 0x0 then buffer is at the top of the stack and that's the address. Otherwise, if it points to another address then to 0x0, the true address of buf is 2 * the number of addresses pointed to before 0x0 away from the top of the stack. The line for buffer should have an address that points to 4 other addresses when its not at the top of the stack. Subtracting the address you get from the address of ebp gives the buffer size + 8 for 2 frames. The payload used is still the same for L1, the 32bit shellcode.


## Task 8: Defeating Address Randomization
### Section 10
![image](https://user-images.githubusercontent.com/46972037/167886947-7e165fb1-658f-46e5-88df-944dae97fcb4.png)

## Task 9: Experimenting with Other Countermeasures
### Section 11.1
I don't really get to see what StackGuard is doing because it specifies to turn off address space randomizatation. Thus, if my exploit.py file has the right values then it will execute the shell on the first attempt. When trying to exit from the shell it keeps going back in though as it continously executes the attack. If exploit.py is wrong and the badfile has the wrong values then it will never be successful.
### Section 11.2
Without the -z execstack flags, call_shellcode.c compiles as normal but executing a32.out or a64.out results in a Segmentation fault.
