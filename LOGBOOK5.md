# LOGBOOK5

This document pretends to document the work that was  during week 5 of the FSI course - [link to guide](https://seedsecuritylabs.org/Labs_20.04/Files/Buffer_Overflow_Setuid/Buffer_Overflow_Setuid.pdf).

## Setting up: 

To work on this guide, we will need to change some settings regarding our shell. There are some countermeasures against buffer overflow attacks that we will ignore, by disabling address space randomization and configuring /bin/sh by linking it to another shell without buffer overflow countermeasures. StackGuard and non-executable stack will be turned off during compilation.

## Task 1

This task's aim is to show how shell-code injections work and what their effects are.

We are given 32 and 64-bit shellcode, that when compiled via makefile, generate two binaries, a32.out and a64.out.

If we run them, we will create a new shell inside our terminal, that will allow us to input commands.

(prints)

## Task 2

The purpose of Task 2 is to understand the vulnerable program stack.c and how to exploit it to gain root privilege.

