# retroarch-cores-aarch64
Compile RetroArch cores for aarch64 devices using WSL2 + Ubuntu 22.04

I have tested this procedure on Windows 10 WSL2 + Ubuntu 22.04.01LTS and on an Anbernic RG28XX device using Knulli Batocera Linux distro.


1. Install WSL and enable WSL version 2 in Windows
2. Install Ubuntu 22.04.01 LTS from the Microsoft Store

From now on, run everything in your Ubuntu terminal:

3. Update and upgrade Ubuntu
```
sudo apt -y update && sudo apt -y upgrade
```

4. Install the necessary packages (*)
```
sudo apt -y install build-essential git libarchive-zip-perl rsync zip python3 python3-pip python3-setuptools python3-wheel ninja-build libopenal-dev premake4 autoconf gcc-aarch64-linux-gnu gcc-arm-linux-gnueabihf g++-aarch64-linux-gnu
```

5. Clone your libretro core (e.g., fuse-libretro)

```
mkdir libretro && cd libretro

git clone --recursive https://github.com/libretro/fuse-libretro.git

cd fuse-libretro
make clean
```

6. Compile the core (4 is the number of CPU procs used by the compiler)
```
CC=aarch64-linux-gnu-gcc CXX=aarch64-linux-gnu-g++ make -j4
```

7. Strip the core
```
aarch64-linux-gnu-strip fuse_libretro.so
```

8. Copy the new core (.so file) to the appropriate folder. In the case of Knulli Batocera, copy it to the SD card in this folder:"
```
/usr/lib/libretro/
```

Now, continue on your device:

9. Test the core

10. Save it permanently using the command line (SSH or similar) on your device:
```
batocera-save-overlay
```


(*) Packages recommended by https://github.com/christianhaitian/retroarch-cores plus g++-aarch64-linux-gnu
