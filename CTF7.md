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
- No PIE, or "No Position Independent Executable", which means that there is no address randomization.

Even though these protections are in place, there are attacks that may still be possible. Some of them are:
- Format strings vulnerabilities;
- Canary bypass, attackers may attempt sophisticated techniques to manipulate or bypass the canary check;
- Data-Only attacks, attacks that manipulate existing data without executing new code might still be feasible.

Next, by analising the source code, we should answer the following questions:

- *Which code line is the vulnerability found?*
    - The vulnerability is on the line 25. The ```scanf()``` in the code scans 32 bytes into the buffer. However the ```load_flag()``` function loads a flag with 40 bytes.

(**print 2**)

- *What does the vulnerability allow?*
    - The vulnerability allows an attacker to overwrite the buffer if they input more than 32 bytes, potentially leading to a buffer overflow. This, in turn, could enable the attacker to manipulate the execution stack, potentially leading to unauthorized access to values and control flow within the program.

- *Which functionality allows us to obtain the flag?*
    - By using a format string and the register where the ```load_flag()``` is called, we might able to access the flag.


### Do the exploit

After answering the questions above, we are able to do the exploit.

Firstly, by running the command ```gdb program``` we are able to obtain the register of the ```load_flag()``` function.

(**print 3**)

Once we determined the address ("\x60\xC0\x04\x08" in format string), we are able to inject the input with the exploit on the "exploit_example.py" and obtain the "flag_placeholder".

(**print 4**)
(**print 5**)

This means that our exploit was successful, therefore we change the LOCAL to False and run the script again. As we can see in the images below, we are able to obtain the value of the flag.

(**print 6**)
(**print 7**)



## Challenge 2

In this challenge, we were given a zip file where we had an executable (program) and a source code (main.c). 

We also had access to the following information below by running the following command:

```SHELL
checksec program
```

(**print 8**)

With this we came to the conclusion that the ```program``` is still without the randomization of addresses, just like the challenge above.

Next, by analising the source code, we should answer the following questions:

- *Which code line is the vulnerability found? What does the vulnerability allow?*
    - The vulnerability is on the line 13. The ```scanf()``` in the code scans 32 bytes into the buffer. However there is no check to make sure the user doens't input more than those bytes. Therefore, this allows an attacker to input more than 32 bytes causing a buffer overflow and possibilly gain access to unauthorized files.

(**print 13**)

- *Is the flag loaded to memory? Or there is a functionality that allows us to have access to the flag?*
    - By analising the code, we realise that the flag is not loaded to the memory. However we have an if clause that checks a key value and if it is the correct one then we get access to the backdoor, which in turn may give us access to the flag.

- *In order to unlock that functionality what do you have to do?*
    - By running the following commands:
        ```SHELL
        gdb program
        p &key
        ```

(**print 9**)


### Do the exploit

After answering the questions above, we had all the information needed to do the exploit.

By analising the source code, we know that the correct value for the key is 0xbeef, which corresponds to the deciaml value 48879. Therefore, we calculated how many bytes we have to print in order to have the correct key value.
We gave a dummy address for padding reasons, which is 4 bytes, and the address of the register we want (the key register in reverse) which is another 4 bytes. Then, we still had to print 48871 bytes (48879-8).

Lastly, as the %n allows us to change the variable, we had everything we needed to execute the exploit.


In order to execute the exploit, we adapted the python code from the challenge above to our challenge.
We deleted the line "p.recvuntil(b"got:")" in order to not cause problems when running the python code.

(**print 12**)

After running the exploit_example.py we got access to the Backdoor and then all we had to do was open the flag.txt file.

(**print 10**)
(**print 11**)