# CTF5

The primary objective of this Capture The Flag (CTF) challenge is to exploit a buffer overflow vulnerability in the program, ultimately gaining unauthorized access to the flag stored in the 'flag.txt' file, which is otherwise inaccessible.

## Challenge 1

In this challenge, we were given a zip file where we had an executable (program), a source code (main.c) and a python script (exploit-example.py).
We also had access to the following information below by running the following command:

```SHELL
checksec program
```

Arch: i386-32-little
RELRO: No RELRO
Stack: No canary found
NX: NX disabled
PIE: No PIE (0x8048000)
RWX: Has RWX segments

We started by analising the source code to understand the following points:

- *Is there any file that is opened and read by the program?*
    - Yes the mem.txt file is opened and read by the program as we can see in the figure below.

(**print 1_1**)

- *Is there any way of controlling the file that is opened?* 
    - Yes, by changing the meme_file variable's content, we can control which file is read and opened.

- *Is there a buffer-overflow vulnerability? If yes, what can you do?*
    - Yes there is. The buffer has 32 bytes but the scanf will read 40. We also know that the meme_file variable has 8 bytes and is positioned before the buffer in memory. Therefore, by overwriting the buffer we can modify the meme_file variable in order to open the file that we want to.

(**print 1_2**)


### Do the exploit

After answering the questions above, we had all the information needed to do the exploit.
By opening the exploit-example.py and changing the DEBUG to True we had the output showned in the images below. 

(**print 1_3**)

(**print 1_4**)

(**print1_5**)

(**print1_6**)

