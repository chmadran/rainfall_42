# rainfall_42

<h4>LEVEL 01</h4>


There's an executable `level1` which cannot be run. 
You can see its strings when you `scp` it on your machine.
Else, you can decompile it or `disas main` in `gdb` but you will only see a call to `main` that calls `get` at a certain address.

With strings, you'll see there's somewhere a "Woops", find it in the decompiler and you see a `run` function that opens a shell. Now, how do you launch that fonction outside of gdb.

Need to do a binary switch of the call to `gets` to the call to `run`.

HOW TO FIND THE OFFSET???

The offset is the distance in bytes from the start of the buffer (where your input begins) to the return address location on the stack.

Calculating the offset to the return address in a buffer overflow exploit is crucial for crafting a payload that precisely overwrites the return address on the stack with the address of the function you want to execute (like run in your case).

Start Small: Begin with a payload that fills the buffer and gradually increases in size. For example, if you know the buffer is around 100 bytes, start with 100 "A"s and then add 4 "B"s to see if you can overwrite the return address.

Analyze Crashes: Run the program under a debugger like gdb, and look at the register used as the return address (EIP on x86) when it crashes. If it's overwritten with your "B"s (0x42424242), you've found the rough location of the return address.

Refine Your Approach: Adjust the number of "A"s and "B"s to pinpoint exactly where the return address begins. This manual binary search helps you identify the offset without automated tools.
