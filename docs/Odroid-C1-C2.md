This is a guide on how to build RetroPie on the Odroid C1/C1+/C2. This is assuming you are starting with a prebuilt image of Ubuntu from HardKernel's Website:

## Download Ubuntu image for Odroid

HardKernel's releases for Ubuntu can be downloaded from <https://wiki.odroid.com/odroid-c2/os_images/ubuntu/ubuntu>.

Write the .img file to your SD card or EMMC module with something like [Win32DiskImager](http://sourceforge.net/projects/win32diskimager/) or [RPI Imager](https://www.raspberrypi.com/software/). The latter can write the `.xz` file directly, while the former needs  a program like [7zip](http://www.7-zip.org/download.html) to extract the `.img` file from the `.xz` compressed archive.  

## Install RetroPie:

Odroid default user is "*root*" with the password "*odroid*".

Preliminary steps (using `pi` as the installation user, but other username can be chosen):

```
apt update && apt upgrade
apt install -y git
useradd -mb /home -s /bin/bash -G input,video pi
echo 'pi ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/pi
passwd pi
```

**NOTE**: When the installation is done using the _20.04 LTS_ Ubuntu image, the `mali-fbdev_20200618-r6p1-2_arm64.deb` package (needed by the installation) will not install cleanly, due to a conflict with the `libegl-dev` package. To resolve the issue, force the overwriting of the conflicting file by installing the `mali-fbdev` package with the command
```
sudo apt-get -o Dpkg::Options::="--force-overwrite" install mali-fbdev
``` 


Installing the RetroPie Setup Script:

Login as "*pi*" user and run

```
cd
git clone --depth=1 https://github.com/RetroPie/RetroPie-Setup.git
cd RetroPie-Setup
sudo ./retropie_setup.sh
```

**Note** if you have issues while compiling modules and it freezes up on you, then you need to tell it to only compile with one core by running the setup script with this:

```
sudo MAKEFLAGS="-j1" ./retropie_setup.sh
```

## Installing Modules

All modules can be installed from the RetroPie Setup Script. First and foremost the two main packages you need in order for the majority of your system to run are RetroArch and EmulationStation:

#### Installing EmulationStation:

- Option 5: Install EmulationStation

Killing X `sudo service lightdm stop`

open new terminal `ctrl+alt+F1`

#### Install EmulationStation Theme

- Option 3: Install Themes

#### Installing RetroArch:

- Option 5: Install RetroArch

## Installing Emulators:

- Option 5: Choose your emulators

## Advanced Configuration

#### AutoStart EmulationStation

- Option 3: Autostart EmulationStation 

#### Rom Transfer

- Option 3: Enable USBRomService

#### Samba Shares

- Option 3: Enable Samba Shares

### Ubuntu 14

#### Boot to Console:

```
sudo -s

sudo echo "manual" >> /etc/init/lightdm.override

```

To start lightdm on command:

```
sudo start lightdm
```

To restore your system so that lightdm is always started on boot:

```
sudo rm /etc/init/lightdm.override
```

#### Disable Screen Blanking in Console:

```
sudo nano /etc/kbd/config
```
comment out

```
BLANK_TIME
POWERDOWN_TIME
```

#### Fix sound not working/stuttering

If you have troubles with no sound or sound stuttering badly in menu or game, check your CPU usage via top or htop. If pulseaudio is using more than 20% then it may be the culprit. In my case, it was using 80%! I had only one sound card output (hdmi) so I completely removed pulseaudio with:

```
sudo apt purge pulseaudio
```

### Ubuntu 16 (systemd)

#### Boot to Console:

    sudo systemctl disable lightdm

To start lightdm on command:

    sudo systemctl start lightdm

To restore your system so that lightdm is always started on boot:
    
    sudo systemctl enable lightdm

#### Auto-login on startup
 
Update visudo editor:

	sudo update-alternatives --config editor

Run visudo:

	sudo visudo

This will open up /etc/sudoers file in editor you configured in first step. Add the following line to the end of the file:

	odroid ALL=(ALL) NOPASSWD:ALL

This tells sudo that the odroid user doesn't need a password. Save and quit.

Then remove your password:

	sudo passwd -d odroid

Test it all worked:

        sudo date

Should give you the date without asking for a password.

Edit the startup of our tty1 (replace with any tty of choice):

	sudo systemctl edit getty@tty1

Add the following text, save and exit

	[Service]
	ExecStart=
	ExecStart=-/sbin/agetty -a odroid --noclear %I $TERM

Finally, restart the whole mess:

        sudo systemctl restart getty@tty1

#### Disable Screen Blanking in Console:

Edit /media/boot/boot.ini with your editor of choice.

Search for:

    no_console_suspend 

and replace it with:

    no_console_suspend consoleblank=0

Finally, run:

    bootini

and reboot for good luck:

    sudo reboot

#### Fix sound not working/stuttering

If you have troubles with no sound or sound stuttering badly in menu or game, try disabling pulseaudio:

    mkdir ~/.pulse
    echo "autospawn=no" >> ~/.pulse/client.conf
    pulseaudio -k

Then reboot, or restart emulationstation.

If the audio still isn't great, stutters or pops or echos, try the following:

Go to: `RetroPie -> RetroArch -> Settings -> Audio`

And set: `Audio Latency (ms) = 192`

Then don't forget to save! `Configurations -> Save Current Configuration`

Feel free to play with the value until it sounds right for you.

#### Fix games running too quickly or inconsistently

Go to: `RetroPie -> RetroArch -> Settings -> Frame Throttle`

And set: `Maximum Run Speed = 1.0x`

Then don't forget to save! `Configurations -> Save Current Configuration`


