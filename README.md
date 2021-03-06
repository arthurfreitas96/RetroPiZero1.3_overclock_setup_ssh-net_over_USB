# RetroPiZero1.3_overclock_setup_net-ssh_over_USB

I recently bought a Raspberry Pi Zero 1.3 and flashed a RetroPie 4.6 image to it's SD card. Testing some emulators using the stock settings, I noticed some expected issues. For example, testing snes9x_2002 (standard emulator for SNES on RPI Zero), I noticed that SuperFX games ran poorly. Some of the other more graphics demanding games, such as Donkey Kong Country 3, suffered with major fps drops and poor audio.

To solve this problem I followed this overclock setup: https://www.raspberrypi.org/forums/viewtopic.php?t=249071.

Then I wanted to install an optional SNES emulator to it, PiSnes, which was developed to run in the RPi Zero. However, since there's no built-in wifi in this board, I had no connection to the Internet. Solving this second problem required following the instructions presented on this link: https://www.slideshare.net/GSHCO/raspberry-zero-usb-in-linux (attention: remember to change the interface names of the commands presented on the slide #15 by your interface names). That way, you can connect the RPi Zero to your Linux PC using only a USB cable, access it via SSH and you can share Internet from your PC to the RPi Zero using only the instructions presented. To access RetroPie with SSH you can run the command 'ssh pi@retropie.local' after plugging in the RPi and connecting to the usb/ethernet network. Connect to it changing the network configuration to use manual IP definition and 192.168.7.1 as address and gateway and 255.255.255.0 as netmask if you followed the tutorial. Also, in your PC, run the following commands, every time after connecting to the RPi, to share Internet (again, remember to name correctly your interfaces):

sudo ifconfig enp0s20u3 192.168.7.1  
sudo iptables --table nat --append POSTROUTING --out-interface wlp6s0 -j MASQUERADE  
sudo iptables --append FORWARD --in-interface enp0s20u3 -j ACCEPT  
sudo sysctl -w net.ipv4.ip_forward=1  

After some research, I found out that lr-gpsp is broken on RetroPie 4.6, but there is a known work around that requires replacing the gpsp_libretro.so with the previous version of the core (4.5), according to the user juchong in https://retropie.org.uk/forum/topic/25111/lr-gpsp-giving-illegal-instruction-on-raspberry-pi-models-0w-3b-and-4/22. Following this method didn't solve my problem and I couldn't make lr-gpsp to work on RetroPie 4.6, so I downgraded to 4.5.1, but kept my overclocking configuration. Now I have gba at full speed in all games I tested until now, with nice sound too, using lr-gpsp.

Right now I'm trying to figure out how to run Super FX and SA-1 games smoothly. Some say that it's not possible in RPi Zero, but I'm actually running Super Mario World 2 Yoshi's Island very well, with minor and rare FPS drops and sound crackling, using my current setup. Star Fox is running at a 10 fps average (this game ran at 15 fps in the original SNES hardware) and Kirby Dream Land 3 is just unplayable (this game used the SA-1 chip) with constant and major fps drops and a lot of sound crackling.

To run this 3 games, I'm using lr-snes9x2002 and the issues of running Super FX and SA-1 games is known by the team developing the emulator and solving it is in the project's to do list.
