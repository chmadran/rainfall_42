# ranfall_42

<h4>LEVEL 00</h4>

![image](https://github.com/chmadran/rainfall_42/assets/113340699/54a5fa96-a968-4b48-a935-99fcc745aa0e)

This is what you see when you open the level0.

<details><summary>SECURITY REPORT DETAILS</summary>
It is a security report relating to the configuration of the OS kernel, and a particular file. 
* GCC Stack Protector Support: Enabled : security feature that helps preventing stack overflow attacks
* Strict User Copy Checks: Disabled : the kernel is not performing stric bound checks on the memory that is copied between user space and kernel space.
* Restrict /dev/mem Access: Enabled : prevents unauthorized users from reading or writing to the system memory.
* Restrict /dev/kmem Access: Enabled : prevents unauthorized access to the kernel memory
* Restrict /dev/kmem Access: Enabled : grsecurity / PaX patches are not applied to the kernel
* Kernel Heap Hardening: No KERNHEAP : no additional hardening measures applied to the kernel's heap management
* System-wide ASLR (kernel.randomize_va_space): Off (Setting: 0) : Address Space Layout Randomization is disabled, meaning the memory addresses used by system and program files are not randomized

The bottom line is then the security details about a specific binary file `/home/user/level0/level0`. 

* No RELRO: Relocation Read-Only (RELRO) is not applied, which means some sections of the binary may be writable that could otherwise be made read-only to prevent overwriting.
* No canary found: Indicates that stack canaries are not used for this binary. Stack canaries are used to detect stack buffer overflow by placing a small, random value (a canary) before critical data like the return address on the stack.
* NX enabled: Non-eXecutable (NX) bit is enabled, meaning that data marked as non-executable cannot be run as code, which helps prevent certain types of exploits.
* No PIE: Position Independent Executable (PIE) is not enabled, meaning the binary is loaded into the same address space each time, making it easier for attackers to guess or infer the location of specific code.
* No RPATH or RUNPATH: Indicates that RPATH and RUNPATH, which specify runtime library search paths, are not used. This is generally a good security practice to avoid loading libraries from untrusted locations.

Now that we know that, I'm not sure how useful it is actually to complete this level. 

</details>

<h4>USING GDB</h4>

Like in level14 of snowcrash, we will need to use GDB here. After downloading the file on our machine (using scp), decompiling it, we can see there is a blocking `if` in the `main`. Therefore, to bypass it, we will change the return value of `atoi`. Return values are stored in the `eax` variable [TODO need to check more]. 

![image](https://github.com/chmadran/rainfall_42/assets/113340699/dc23703d-c4b2-4f51-a323-f70deb389724)


Using `gdb`, we can launch the binary, disasemble main, set a breakpoint after atoi returns and change the value to one that will allow us to bypass the `if`, in our case it's the int `423`, or `0x1a7`. 

The set of commands used is : 
* `gdb level0`
* `b main`
* `run [random argument??]`
* `disas main`
* `b [address of eax right after atoi under format *0x00000]`
* `continue`
* `set $eax = 423`
* `continue`

Now TBC : find what needs to be passed as argument to "exploit" a vulnerability in level0 after bypassing the if protection - think about the security brief at the beggining ? 
