# LOGBOOK4

This document pretends to document the work that was  during week 4 of the FSI course - (link to guide) [https://seedsecuritylabs.org/Labs_20.04/Files/Environment_Variable_and_SetUID/Environment_Variable_and_SetUID.pdf]

## Task 1: Manipulating Environment Variables

In this task, the goal was to confirm what environment variables exist.

```SHELL
printenv # we could also use env
```

(print do output)

Or, if we want, we could also use the command for checking one environment variable specifically, in this example, we are using variable PWD as an example:

```SHELL
env PWD # we could also use printenv
```

(print do output)

## Task 2: Passing Environment Variables from Parent Process to Child Process

The next task was to compile the code that creates a new process. This new process (the child) becomes a duplicate of the parent process (we used fork() function).

After compiling the code and save them into different files, we found out that the outputs were the same, which concludes that, using the fork method, the envirnoment variables of the child process are the same as the ones that are in the parent envirnoment variable.

```SHELL
gcc myprintenv.c
a.out > file
```

(print do output)

## Task 3: Environment Variables and execve()

This task studies how environment variables are affected when a new program is executed via execve(), a function that runs the new program inside a system calling process.

After compilation of the given code, we can see that there is no return. However, after changing NULL to environ, we can see that the environment variables are returned. This is due to the fact that environ is defined by unistd.h and is the list of environment variables of the system, that are inherited by the program.

```SHELL
gcc myenv.c -o myenv
./myenv
```
(print do output)

## Task 4: Environment Variables and system()


## Task 5: Environment Variables and Set-UID Programs


## Task 6: The PATH Environment Variable and Set-UID Programs
