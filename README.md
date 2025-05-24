## FLAG - PClub{4lw4ys_cl05e_y0ur_fil3s}

Well this challenge was not that hard despite me panicking I managed to do it in like 30 minutes.

first connected to the server with netcat. Then used 'ls' to list all the contents in that directory.

file_chal and file_chal.c came up 

using 'cat' opened file_chal.c and it was this

Then doing 'ls -l' told me the permissions of all the files in the directory 

tried running 'chmod +x file_chal' but it was pointless since the file was already executable.

after re-reading the .c file

```
#include <fcntl.h>
#include <unistd.h>

int main () {
    int fd = open ("/root/flag", 0);

    // Dropping root previliges
    // definitely not forgetting anything
    setuid (getuid ());

    char* args[] = { "sh", 0 };
    execvp ("/bin/sh", args);
    return 0;
}
```

we can see the code is executing a shell, and there is a file descripter involved.
now, we know that 0 -> stdin, 1->stdout, 2->stderr, so, since the fd is never really closed, 
running cat <&3 gave me the flag.

![alt text](/Assets/WEB2_PHOTO1.jpg)