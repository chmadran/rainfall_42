# rainfall_42

<h4>LEVEL 01</h4>


There's an executable `level1` which cannot be run. 
You can see its strings when you `scp` it on your machine.
Else, you can decompile it or `disas main` in `gdb` but you will only see a call to `main` that calls `get` at a certain address.

With strings, you'll see there's somewhere a "Woops", find it in the decompiler and you see a `run` function that opens a shell. Now, how do you launch that fonction outside of gdb.

Need to do a binary switch of the call to `gets` to the call to `run`.

<h4>ANATOMY OF MEMORY</h4>

![image](https://github.com/chmadran/rainfall_42/assets/113340699/2486f282-48db-4bec-af68-aa069e81c39c)

And the Stack...

![image](https://github.com/chmadran/rainfall_42/assets/113340699/d6d2eb77-881c-4b7e-be93-95d1dd5e5450)

Vuffer Space fills up with characters, it grows downward. If you sent characters at it like a bunch of As, you should reach the EBP and stop. When there's a buffer overflow attack, you reach over the EBP and into the EIP : 

![image](https://github.com/chmadran/rainfall_42/assets/113340699/2d36c069-cb2d-4c3c-a3e0-96425491d361)

Use this address to point to directions we instruct. 

HOW TO FIND THE OFFSET???

The offset is the distance in bytes from the start of the buffer (where your input begins) to the return address location on the stack.

Calculating the offset to the return address in a buffer overflow exploit is crucial for crafting a payload that precisely overwrites the return address on the stack with the address of the function you want to execute (like run in your case).

Start Small: Begin with a payload that fills the buffer and gradually increases in size. For example, if you know the buffer is around 100 bytes, start with 100 "A"s and then add 4 "B"s to see if you can overwrite the return address.

Analyze Crashes: Run the program under a debugger like gdb, and look at the register used as the return address (EIP on x86) when it crashes. If it's overwritten with your "B"s (0x42424242), you've found the rough location of the return address.

Refine Your Approach: Adjust the number of "A"s and "B"s to pinpoint exactly where the return address begins. This manual binary search helps you identify the offset without automated tools.

----------------

info registers (post running to get return address)
look at eip -> return pointer 

Ressources: 
- https://www.youtube.com/watch?v=1S0aBV-Waeo - explanation on buffer overflow attack
- https://www.cobalt.io/blog/pentester-guide-to-exploiting-buffer-overflow-vulnerabilities

Important concepts 

Registers: Registers are fast, small storage locations in a processor for storing data that is being processed. Using buffer overflows, attackers can overwrite the contents of certain registers, allowing them to control the flow of execution.

EAX: The EAX register is a general-purpose register that holds the result of arithmetic operations and stores the address of the processed data. In buffer overflow exploitation, the EAX register stores the address of a pointer to the input buffer that contains the attacker's malicious code.

EDX: The EDX register is another general-purpose register often used with the EAX register. The EDX register stores the address of the fixed-size buffer that is being overflowed.

EBP: The EBP register is used as a base pointer to locate data on the stack. In buffer overflow exploitation, the EBP register can locate the return address stored on the stack, which the attacker will overwrite with the address of their malicious code.

ESP: The ESP register is used as a stack pointer to keep track of the top of the stack. In buffer overflow exploitation, the ESP register can locate the buffer that is being overflowed and identify the adjacent memory locations that will be overwritten.

EIP: The EIP register holds the address of the next instruction to be executed by the program. In buffer overflow exploitation, the attacker will attempt to overwrite the contents of the EIP register with the address of their malicious code.

-----------------------------------------------

OK so to find the offset, I sent a list of As followed by the alphabet to the program and ran it a million times until i saw a letter different from 'A' in the EIP. Finally, I get "HIJK" where :

* 0x48 corresponds to H
* 0x49 corresponds to I
* 0x4A corresponds to J
* 0x4B corresponds to K

![image](https://github.com/chmadran/rainfall_42/assets/113340699/fff3555a-b46e-4d69-8f9b-682f621407a0)

Your input: "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABCDEFGHIJKLMNOPQRSTUVWXYZ"

Looking at the alphabet part: "HIJK" are the 8th to 11th characters in the alphabet sequence you added after the "A"s. If we assume all preceding characters are "A"s, you can calculate the offset to the start of "HIJK" as follows:

Count the number of "A"s: 65 (based on your input shown in the echo command).

Add the position of "H" in your alphabet sequence, which starts immediately after the "A"s: 7 (A=1, B=2, ..., H=8, but since we're starting counting at 0 for the position in the sequence after "A"s, "H" is at position 7).

So, the offset where "HIJK" starts, and thus the point at which the eip was overwritten, is 65 (number of "A"s) + 7 (position of "H" in the sequence after the "A"s) = **72.**

Ok actually the perfect offset is 76 as shown here cest incomprehensible lol 

![image](https://github.com/chmadran/rainfall_42/assets/113340699/5de3c128-5fa9-4ecc-a72c-d54731cf15cf)

  ----------------------------------------------------------------------------

The - used with cat, it waits for additional input from stdin (i.e., your keyboard input). Whatever you type after executing the command will be appended to the content of payload. To signal the end of your input from stdin, you would press Ctrl+D (EOF in Unix/Linux environments) if you're manually entering additional data. If you don't input anything and immediately press Ctrl+D, it just means no additional data is appended to the content of payload.

![image](https://github.com/chmadran/rainfall_42/assets/113340699/b9f175f4-f4d5-44e6-bcf9-0550543f79aa)


  ```53a4a712787f40ec66c3c26c1f4b164dcad5552b038bb0addd69bf5bf6fa8e77```

  You only needed to leave `stdin` open.....
