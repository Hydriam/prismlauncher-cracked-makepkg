# prismlauncher-cracked-makepkg
A **unofficial** makepkg repo (something like packages on aur) for [prismlauncher-cracked](https://github.com/Diegiwg/PrismLauncher-Cracked) project.
This is used to simply build prismlauncher-cracked arch package using makepkg. 
It can be used on Arch Linux (and based like Manjaro).

This is not endorsed by Prism Launcher or by Prism Launcher Cracked Projects.

This project doesnt distribute prismlauncher-cracked binaries.
This project just has build files for easier building of prismlauncher-cracked package, with the package you can install it to system.

## Instructions
If you dont have base-devel package installed, install it: 
```
sudo pacman -S base-devel
```
Then get files to build the package:
```
git clone https://github.com/Hydriam/prismlauncher-cracked-makepkg
cd prismlauncher-cracked-makepkg
```
Then we build it, in makepkg -s parametr installs dependencies and -i installs the package to the system after building.
```
makepkg -si 
```
