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
We will use the command ```echo hello | nc 10.9.0.5 9090```
If myprintf() returns, it will print "Returned properly" on the terminal.

(print)

## TASK 2.A

To accomplish this task, understanding how printf works in terms of exposing sensitive data from the stack is crucial. For starters, we can incorporate 4 bytes into the code to determine the address of the next variable. These 4 bytes will serve as a reference point for our manipulation. However, it's very important to adjust the format string to successfully print the variable.

Given that we aim to print 4 bytes, each consisting of 8 bits, we need to print 64 (4 * 8) registers, with 63 preceding registers leading up to the target register. Therefore, we'll need to modify the string 's' to accommodate this format.

(print ficheiro)

We then execute build_string.py, and run ```cat badfile | nc 10.9.0.5 9090```

And as we can see, we have the desired result, as we know the next address:

(print)

## TASK 2.B

Now that the address of the next variable has been identified, we can use it to access the content stored in the heap. Our objective here is to set the value of the variable 'number' to a specific address provided by the server, the 'secret address' (0x080b4008).

This time we aim to retrieve the content stored at this address. We know that this content is in string format. Therefore, our approach involves reading 63 registers and, for the last one,using the '%s' format specifier to read the content in the register.

By running ```cat badfile | nc 10.9.0.5 9090``` again, we now get the secret message:

(print output)


## TASK 3.A

In this task, our goal is to modify the target variable. It's address is 0x080e5068

Similar to what we did in task 2B, we aim to manipulate the address by positioning it at the start of our string.

To access this address, we'll use the "%x" format specifier 64 times, altering the values for 63 iterations before using "%n". This method is similar to the one we previously used to disrupt the server, allowing us to write to the address that the pointer is referencing. In this scenario, the pointer leads to the location where the variable was originally stored.

(print output)

## TASK 3.B

In task 3.B, we have to change the target variable's value to 0x5000.

We know that using "%n" in 'printf' lets us write a number of characters printed so far. This time, we're aiming for 0x5000. To make this work, our string needs to be 5000 characters long before we reach "%n".

(foto do build_string)

After which, we get the following output:

(print do output)

## Conclusions

This lab allowed us to understand the versatility and importance of the printf function, while also showing us that we have to be very careful while using it.
