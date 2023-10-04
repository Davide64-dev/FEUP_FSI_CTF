# LOGBOOK4

This document pretends do document the work that was made (adicionar link para o enunciado)

## Task 1: Manipulating Environment Variables

In this task, the goal was to confirm which are the envirnoment variables that exist.

```SHELL
printenv # we could also use env
```

(print do output)

Or, if we want, we could also use the command for checking one envirnoment variable specifically, in this example, we are using variable PWD as an example:

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
