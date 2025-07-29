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
sudo apt -y install build-essential git libarchive-zip-perl rsync zip python3 python3-pip python3-setuptools python3-wheel ninja-build libopenal-dev premake4 autoconf gcc-aarch64-linux-gnu g++-aarch64-linux-gnu
```
(*) Packages recommended by https://github.com/christianhaitian/retroarch-cores , adding g++-aarch64-linux-gnu and removing gcc-arm-linux-gnueabihf

5. Clone your libretro core (e.g., fuse-libretro)

```
mkdir libretro && cd libretro

git clone --recursive https://github.com/libretro/fuse-libretro.git

cd fuse-libretro
make clean
```

6. Compile the core
```
CC=aarch64-linux-gnu-gcc CXX=aarch64-linux-gnu-g++ make -j$(nproc) 
```

7. Strip the core
```
aarch64-linux-gnu-strip fuse_libretro.so
```

8. Copy the new core (.so file) to the appropriate folder. In the case of Knulli Batocera, copy it to the SD card in this folder:
```
/usr/lib/libretro/
```

Now, continue on your device:

9. Test the core

10. Save it permanently using the command line (SSH or similar) on your device:
```
batocera-save-overlay
```

# New cores

If you want to add a new core, you will need to do these steps:

1. Copy the new core (.so file) to the appropriate folder. In the case of Knulli Batocera, copy it to the SD card in this folder:
```
/usr/lib/libretro/
```
Example: /usr/lib/libretro/genesisplusgx-paprius_libretro.so 

2.- Copy the .info file associated to the core to the appropriate folder. In the case of Knulli Batocera, copy it to the SD card in this folder:
```
/usr/share/libretro/info/
```
Example: /usr/share/libretro/info/genesisplusgx-paprium_libretro.info

3.- Edit the EmulationStation configuration to add the new core:
File:
```
/usr/share/emulationstation/es_systems.cfg
```

Example:
```
  <system>
        <fullname>Mega Drive</fullname>
        <name>megadrive</name>
        <manufacturer>Sega</manufacturer>
        <release>1988</release>
        <hardware>console</hardware>
        <path>/userdata/roms/megadrive</path>
        <extension>.bin .gen .md .sg .smd .zip .7z</extension>
        <command>emulatorlauncher %CONTROLLERSCONFIG% -system %SYSTEM% -rom %ROM% -gameinfoxml %GAMEINFOXML% -systemname %SYSTEMNAME%</command>
        <platform>genesis, megadrive</platform>
        <theme>megadrive</theme>
        <group>megadrive</group>
        <emulators>
            <emulator name="libretro">
                <cores>
                    <core default="true">genesisplusgx</core>
                    <core>genesisplusgx-wide</core>
                    <core>genesisplusgx-paprium</core>
                    <core>picodrive</core>
                </cores>
            </emulator>
        </emulators>
  </system>
```
4.- Save changes
```
batocera-save-overlay
```
You may need to restart EmulationStation or reboot the system.


# Packages precompiled

You can find the following precompiled cores in https://github.com/jfroco/retroarch-cores-aarch64/tree/main/cores :

- https://github.com/libretro/fuse-libretro
- https://github.com/libretro/blueMSX-libretro
- https://github.com/libretro/a5200
- https://github.com/libretro/libretro-atari800

  


