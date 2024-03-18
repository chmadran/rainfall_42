# ranfall_42

<h4>LEVEL 01</h4>


There's an executable `level1` which cannot be run. 
You can see its strings when you `scp` it on your machine.
Else, you can decompile it or `disas main` in `gdb` but you will only see a call to `main` that calls `get` at a certain address.

With strings, you'll see there's somewhere a "Woops", find it in the decompiler and you see a `run` function that opens a shell. Now, how do you launch that fonction outside of gdb.

Need to do a binary switch of the call to `gets` to the call to `run`.

