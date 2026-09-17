## WSL, linux and gp2c on Win11

Yesterday I wanted to compile a PARI/GP program to C. However, gp2c is only available as source.

I fetched it, but then had trouble to compile it. I did have MinGW and Msys2 installed, 
and (weird enough) I also had a gcc command under Msys2 UCRT64, but not under Msys2 MSYS, MINGW64, CLANG64...

### installation of WSL and libpari-dev

But there I also lacked the dev utils & include files, so I decided to install WSL.
For the base system that was a cake, I simply typed `wsl` (or so).  
But then I couldn't install a distro until the next reboot, so I switched to another task. 
"Luckily"(?), on the next day Windows decided somewhere around mid-day that it should reboot (without asking...),
so I could finally `wsl --install Debian`).

But then I didn't know the root password, but luckily (?) you can use `wsl -u root` 
to log in as root from the Win11 CLI (e.g., PowerShell).
So I could add my normal user account to the sudo group with `usermod -aG sudo mhasler`.

Then I could finally (even without `sudo`) `apt install -y build-essential pari-gp libpari-dev`.
That required about 150 packages, but OK.
### gp2c
Then I could login again from my normal Win account, and I landed in my Windows home dir,
/mnt/c/Users/myself/. So I could easily go the the directory with my .gp file.
There, `gp2c MyFile.gp` spat all out on the terminal, so I used `-o MyFile.c`.
Then,
    gcc -O3 -fPIC -shared -I/usr/include/pari MyFile.c -o MyFile.so -lpari
went smoothly(*). I learned that the necessary `install()` commands are given in the header of the .c file!

Unfortunately, I couldn't "load the library" `Myfile.so` in the PARI/GP session on Windows, but in the WSL shell,
    > ? GP;install("init_MyFile","v","init_MyFile","./PiGame.so");
worked. IDK what that exactly does, I had to run all the other `install` commands in order to use those functions --
one per function, and a few `anon` functions in addition to what I defined in my code -- 
probably some closures that were "inlined" in my code)

### aftermath
Weirdly enough, on the next day, I had to face several new obstacles that appeared out of nowhere.

In one compilation run, the "init_..." was the only "install" line in `MyFile.c` -- maybe due to a compilation error occurring somewhere, that problem disappeared by itself.

BUT, the main problem is that I faced an error that would not disappear:
        MyFile.c:507:3: error: implicit declaration of function ‘andpari’; did you mean ‘addri’? [-Wimplicit-function-declaration]
      507 |   andpari(stoi(/* ... */
          |   ^~~~~~~
After many fruitless investigations, the final fix was to add `#include <pari/paripriv.h>` **after** `#include <pari/pari.h>` in `MyFile.c`, but since that would require manual editing after each compilation, I added `-include pari/paripriv.h` in the compiler flags -- but since then it's seen before the `#include <pari/pari.h>`, I also had to add `-include pari/pari.h` **before that** to the compiler flags.

(Still not knowing which tool might use the `/*-*- compile-command: ...*/` in the header, I finally created a `Makefile` to remember these (and all other) compiler flags.
