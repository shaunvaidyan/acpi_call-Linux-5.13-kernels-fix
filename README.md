# acpi\_call 1.2.2 tutorial for fixing crashes with TLP and modern Thinkpads on Fedora and Ubuntu distros.
This is a fork of [acpi_call](https://github.com/mkottman/acpi_call) maintained by the NixOS community.

### acpi_call is the source of many crashes and freezing in my Fedora 35 install on my Thinkpad T480

My Thinkpad T480 running Fedora 35 was constantly freezing and rebooting. Naturally, I use the TLP package for power management and setting battery threshold levels. I was able to trace the source of those crashes with a bug interaction between one of TLP's dependencies (acpi_call < 1.2.2) and linux kernals 5.13+. The acpi_call 1.2.2 package fixes the issues but the main repos for Canonical (Ubuntu) and Fedora builds package the acpi_call package from a dated source version of 1.2.1. The NixOS community produced an updated fork of the acpi_module so here is a tutorial for compiling and installing the module that I've used to address my issues.

### Tutorial  
1. First remove previously installed acpi-call-dkms package (if any)  
``sudo apt purge acpi-call-dkms`` (Ubuntu)  
``sudo dnf remove acpi_call`` (Fedora)  

2. Install git & dkms (if you don’t have it installed yet)  
``sudo apt install git dkms``  
``sudo dnf install git dkms``  

3. Clone the repository at nix-community/acpi_call  
``git clone https://github.com/nix-community/acpi_call.git``  

4. Navigate to cloned repo  
``cd acpi_call``  

5. Prepare dkms.conf file  
``make dkms.conf``  

6. Copy the module source to the shared sources directory  
``sudo cp -R . /usr/src/acpi-call-1.2.2``  

7. Add the module to the dkms tree for build  
``sudo dkms add -m acpi-call -v 1.2.2``  

8. Build the module  
``sudo dkms build -m acpi-call -v 1.2.2``  

9. Install the module  
``sudo dkms install -m acpi-call -v 1.2.2``  

10. Reboot  
``sudo reboot``  

### Notes on dkms and Secure Boot

If that is the way you want to install this module, you can follow 
[this guide](https://web.archive.org/web/20210215173902/https://gist.github.com/dop3j0e/2a9e2dddca982c4f679552fc1ebb18df) ([mirror](https://gist.github.com/s-h-a-d-o-w/53c2215e955c3326c6ec8f812a0d2f27))
to have dkms automatically sign the module after building.


***

Copyright (c) 2010: Michal Kottman

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.
