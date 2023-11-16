# LOGBOOK 7

This document intends to document the work that was done during week 7 of the FSI course - [link to guide](https://seedsecuritylabs.org/Labs_20.04/Files/Format_String/Format_String.pdf).

## Setting Up

To correctly perform the tasks in the guide, we first need to turn off ASLR:

```
sudo sysctl -w kernel.randomize_va_space=0
```

When compiling our vulnerable program, we need to do so using the ```-z execstack``` option, allowing us to execute code on the stack.

We also need to setup docker, using the following commands:

```
$ docker-compose build # Build the container image
$ docker-compose up # Start the container
```

and then opening another terminal and writing:
```
$ dockps
$ docksh <id> //id is given when you start the docker container
```

## TASK 1

In this task, our goal is to crash the program by providing an input to the server.
We will use the command ```echo hello | nc 10.9.0.5 9090.```
If myprintf() returns, it will print "Returned properly" on the terminal.

(print)

## TASK 2

## TASK 3.A

## TASK 3.B
