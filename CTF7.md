# CTF7 - Format Strings

The objective of this CTF is to delve into the vulnerability associated with format strings. Specifically, the focus is on the ability to read and write to arbitrary memory addresses, extending beyond the confines of the stack. The ultimate goal is to successfully access the flag stored within the flag.txt file.

## Challenge 1

In the first challenge, we are given a zip file containing an executable (program), the source code (main.c) and a python script (exploit_example.py).
We also had access to the following information below by running the following command:

```SHELL
checksec program
```

(**print 1**)

Here we obtain the following information:
- Partial RELRO, short for "RELocation Read-Only," enhances security by restricting write access to specific memory sections after dynamic linker address resolution. This measure fortifies the system against unauthorized write operations in critical binary areas.
- The active canary serves as a defense mechanism against buffer overflow attacks. If such an attack occurs, the canary is likely to be overwritten. Subsequently, the program can verify the integrity of the canary, enabling the detection of potential buffer overflow attempts.
- Enabled NX, or "No eXecute," is a security feature preventing the execution of code in designated memory regions intended for data storage.

Even though these protections are in place, there are attacks that may still be possible. Some of them are:
- Format strings vulnerabilities;
- Canary bypass, attackers may attempt sophisticated techniques to manipulate or bypass the canary check;
- Data-Only attacks, attacks that manipulate existing data without executing new code might still be feasible.

Next, by analising the source code, we should answer the following questions:

- *Which code line is the vulnerability found?*
    - The vulnerability is on the line 25. The ```scanf()``` in the code scans the buffer which has 32 bytes. However the ```load_flag()``` function loads a flag with 40 bytes.

(**print 2**)

- *What does the vulnerability allow?*
    - The vulnerability allows an attacker to overwrite the buffer if they input more than 32 bytes, potentially leading to a buffer overflow. This, in turn, could enable the attacker to manipulate the execution stack, potentially leading to unauthorized access to values and control flow within the program.

- *Which functionality allows us to obtain the flag?*
    - By using a format string and the register where the ```load_flag()``` is called, we might able to access the flag.


### Do the exploit

After answering the questions above, we are able to do the exploit.

Firstly, by running the command ```gdb program``` we are able to obtain the register of the ```load_flag()``` function.

(**print 3**)

Then, with the register value, we are able to insert it on the "exploit_example.py" in format string: "\x60\xC0\x04\x08" and obtain the "flag_placeholder"

(**print 4**)
(**print 5**)

This means that our exploit was successful, therefore we change the LOCAL to False and run the script again. As we can see in the images below, we are able to obtain the value of the flag.

(**print 6**)
(**print 7**)

