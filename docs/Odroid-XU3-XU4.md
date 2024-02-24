
This is a guide on how to build RetroPie on the Odroid XU3/XU4. This is assuming you are starting with a prebuilt image of Ubuntu 16.04 Minimal from HardKernel's Website:

## Download Ubuntu 18.04.2 Minimal Image for Odroid XU3/XU4:

### The XU4 is fully software compatible with XU3!

Installation images for Odroid are found at <https://odroid.in/?directory=.%2Fubuntu_18.04lts%2F>    
Direct link to the Ubuntu 18.04 image - [here](https://odroid.in/ubuntu_18.04lts/XU3_XU4_MC1_HC1_HC2/ubuntu-18.04.1-4.14-minimal-odroid-xu4-20181203.img.xz).

Extract the .xz file with a program like [7zip](http://www.7-zip.org/download.html).    
Write the .img file to your SD card or EMMC module with something like [Win32DiskImager](http://sourceforge.net/projects/win32diskimager/)

## Install RetroPie:

### Preliminary steps:

1. As the fresh install of Ubuntu Minimal includes only the root user (password: odroid) a new user has to be created:
```
adduser NameOfYourChoice
```
2. Add new created user to the sudo group:
```
usermod -a -G sudo NameOfYourChoice
```
3. Upgrade system:
```
sudo apt update
sudo apt upgrade
sudo apt full-upgrade
shutdown -r now
```
4. Set the locale settings. Below you can find an example for American English:
```    
apt install language-pack-en-base
update-locale LC_ALL="en_US.UTF-8"
update-locale LANG="en_US.UTF-8"
update-locale LANGUAGE="en_US.UTF-8"
shutdown -r now
dpkg-reconfigure locales
```
5. Check if all locale variables were correctly set by using the "locale" command. Below you will find an exemplary output: 
```
LANG=en_US.UTF-8
LANGUAGE=en_US.UTF-8
LC_CTYPE="en_US.UTF-8"
LC_NUMERIC="en_US.UTF-8"
LC_TIME="en_US.UTF-8"
LC_COLLATE="en_US.UTF-8"
LC_MONETARY="en_US.UTF-8"
LC_MESSAGES="en_US.UTF-8"
LC_PAPER="en_US.UTF-8"
LC_NAME="en_US.UTF-8"
LC_ADDRESS="en_US.UTF-8"
LC_TELEPHONE="en_US.UTF-8"
LC_MEASUREMENT="en_US.UTF-8"
LC_IDENTIFICATION="en_US.UTF-8"
LC_ALL=en_US.UTF-8
```
6. Update visudo editor:
```
sudo update-alternatives --config editor
```
7. Enable NOPASSWD for the new created user:
```
sudo visudo
```
This will open up /etc/sudoers file in editor you configured in first step. Add the following line to the end of the file:
```
NameOfYourChoice ALL=(ALL) NOPASSWD:ALL
```
This tells sudo that the NameOfYourChoice user doesn't need a password. Save and quit.

8. Remove password for the new created user:
```
sudo passwd -d NameOfYourChoice
```
Test it all worked:
```
sudo date
```
Should give you the date without asking for a password.

9. Edit the startup of our tty1 (replace with any tty of choice):
```
sudo systemctl edit getty@tty1
```
Add the following text, save and exit
```
[Service]
ExecStart=
ExecStart=-/sbin/agetty -a NameOfYourChoice --noclear %I $TERM
```
Finally, restart the whole mess:
```
sudo systemctl restart getty@tty1
```
10. Disable Screen Blanking in Console and set the CPU Governor Setup:

Edit /media/boot/boot.ini with your editor of choice and add before "setenv bootargs":
```
setenv RetroPie "no_console_suspend consoleblank=0"

# --- CPU Governor Setup ---
# Uncomment only one line. New governor is set after 90secs after boot.
# ------------------------------------------
# - Performance (Keep all the CPU's at Maximum frequency)
# setenv governor "performance"
# ------------------------------------------
# - Ondemand (recommended)
setenv governor "ondemand"
```
and add the value of the new created variable to "setenv bootargs":
```
setenv bootargs "${bootrootfs} ${videoconfig} ${hdmi_phy_control} ${hud_quirks} smsc95xx.macaddr=${macaddr} ${external_watchdog} governor=${governor} ${RetroPie}"

```
11. Reboot and log in as the new created user (NameOfYourChoice):
```
sudo reboot
```
12. Install libsdl2-dev and xorg
```
sudo apt install libsdl2-dev xorg
```
13. Install the RetroPie Setup Script (logged in as NameOfYourChoice)
```
cd
sudo apt install git
git clone --depth=1 https://github.com/RetroPie/RetroPie-Setup.git
```
14. Run the Setup Script:
```
cd RetroPie-Setup
sudo ./retropie_setup.sh
```

**Note** if you have issues while compiling modules and it freezes up on you, then you need to tell it to only compile with one core by running the setup script with this:

```
sudo MAKEFLAGS="-j1" ./retropie_setup.sh
```
### Optional steps:
A. Adjust your time and timezone:
```
sudo dpkg-reconfigure tzdata
```
B. Upgrade the kernel:
```
sudo apt update
sudo apt upgrade
sudo apt full-upgrade
sudo apt install linux-image-xu3
shutdown -r now
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
