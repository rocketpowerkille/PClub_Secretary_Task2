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




## FLAG - PClub{Franklin_Reiter_is_cool}

So started off by connecting with netcat to the given server

Got the prompt "Find a string such that SHA-256 hash of "SMJeOh" concatenated with your input starts with the the string "30073"."

and the prompt changed everytime u connected with netcat.

we will now use this code to get the final string

```
import hashlib

def find_string_with_hash_prefix(prefix, base_str="SMJeOh"):
    i = 0
    while True:
        test_str = base_str + str(i)
        hash_result = hashlib.sha256(test_str.encode()).hexdigest()
        if hash_result.startswith(prefix):
            return str(i), hash_result
        i += 1


prefix = "30073"
print("Searching for a string that produces a hash starting with", prefix)

result_str, result_hash = find_string_with_hash_prefix(prefix)

print(f"Found solution: '{result_str}'")
print(f"Hash: {result_hash}")
```

on inputting the answer we get three options whether to get the 1st part of the code or the 2nd part of the code or the flag itself which was just a hoax and the server asked for a padding.

we first get both the parts of the code by connecting with netcat twice and then on the third try we go for the padding

we can encrypt any message with our own padding such that:
c = ((m+p)^3)%n where p is sha256(padding)

If we give two different paddings, we can form two different equations

c1 = ((m+p1)^3) %n
c2 = ((m+p2)^3) %n

Now I assumed ((m+p1)^3) and ((m+p2)^3) to be less than n as a test but it ended up being correct(but for some reason directly finding the cuberoot did not work),

c2 -c1 = (m+p2)^3 - (m+p1)^3

we know p2 and p1, so we can say d = p2-p1, and a = m+p1

therefore, c2-c1 = (a+d)^3 - a^3
c2-c1 = 3da^2 + 3ad^2 + d^3

if we rearrange the equation, we get

(c2 - c1- d^3)/(3d) = a^2 + da

or K = a^2 + da where K = (c2 - c1- d^3)/(3d)

a^2 + da - K = 0 is a quadratic equation and we can solve for a using the quadratic formula

since a = m+p1, m = a-p1 and that is the message

so scripting the code for that

```
from hashlib import sha256
from Crypto.Util.number import long_to_bytes, isPrime, inverse
import math

u1 = b"0"
u2 = b"1"

p1 = int(sha256(u1).hexdigest(), 16)
p2 = int(sha256(u2).hexdigest(), 16)
d  = p2 - p1

c1 = 13437526472436443794216183194447347160957723113505232847990537484597214364461254725236231578028409205628532896102373976466083683889840534339119282416411100447318359966949824006689421404186742348951798174294813055116705263039935981529841118559344321945774780626487039435081178264499545778522980216616855019749731586010568502666752167352049568778673364831627346665954746461688496049302648328679930025585624544249598391555764547171626928540386957957549518841911537616722431465187008642766449369067365097076507474993328806567685674699045933763172208862899614531491969200226912337212523724436528753345466586891660131401121 # paste ciphertext for u1
c2 = 13437526472436443794216183194447347160957723113505232847990603147292226928038102057351088581769825769065742799938562195899137207985168638686932973500805175120244776171552623797311717352445842354506839648768961557995566066583379882671063443061221126889161415626667882853789182863427348340550703864877348720316075406615895429590542123490206825841826084125675586562000477548391938871164802515094842964422894509501874136226610585205845061872299086431519078988424124896067831101905411828982263797227188944518022431818652500299284644830387239226510273289074636855097814995843282536891849770922688308317857032217413503938121

Delta = c2 - c1
K = (Delta - d**3) // (3*d)
disc = d*d + 4*K
sqrt_disc = int(math.isqrt(disc))
a = (-d + sqrt_disc) // 2

m_int = a - p1
flag = long_to_bytes(m_int)
print(flag)
```

and here comes the flag
```
Nice work breaking through this SECURE SYSTEM! You deserve this flag: PClub{Franklin_Reiter_is_cool}
```