title: WSL & linux on Win11
---
## WSL & linux on Win11

Yesterday I wanted to compile a PARI/GP program to C. However, gp2c is only available as source.

I fetched it, but then had trouble to compile it. I did have MinGW and Msys2 installed, 
and (weird enough) I also had a gcc command under Msys2 UCRT64, but not under Msys2 MSYS, MINGW64, CLANG64...

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

Then I could login again from my normal Win account, and I landed in my Windows home dir,
/mnt/c/Users/myself/. So I could easily go the the directory with my .gp file.
There, `gp2c MyFile.gp` spat all out on the terminal, so I used `-o MyFile.c`.
Then,
    gcc -O3 -fPIC -shared -I/usr/include/pari MyFile.c -o MyFile.so -lpari
went smoothly. I learned that the necessary `install()` commands are given in the header of the .c file.

Unfortunately, I couldn't "load the library" `Myfile.so` in the PARI/GP session on Windows, but in the WSL shell,
    > ? GP;install("init_MyFile","v","init_MyFile","./PiGame.so");
worked.

Weirdly enough, in a later compilation run, the "init_..." was the only "install" line in `MyFile.c` -- under investigation!


