Tutorial Summary
---

## Chapter 0 Raspberry Pi Preparation

FNK0043 get tutorial
http://freenove.com/fnk0043

Github: 
https://github.com/Freenove/Freenove_4WD_Smart_Car_Kit_for_Raspberry_Pi

### Raspberry Pi OS
Pg28
- www.raspberry.org/downloads/   >  Installed Raspberry Pi Imager
- Select
  - Raspberry Pi Device:  Raspberry Pi 4
  - Operating System: Raspberry Pi Os (64-bits)
  _ Storage : <32G micro SD>
- Edit Setting > Set hostbame, username , Ssid, password (need to type over) > enable ssh
- Put SD card into RPI board
- Green blinking, shown that is reading

Ref from pg42: 
https://youtu.be/3VexTFHZUSY

My ref: https://youtu.be/d5AheGVw2yo?si=OaosI8-6qHfpslQs

### Test from other laptop, `cmd`
pg36. Windows > Win+r > cmd
- `$ ping -4 raspberrypi` > shown `192.168.3.110`
- `$ ssh pi@192.168.3.110` and password
- now u r in the RPI!

### Setup software
from pg 42 
- Using monitor, keyboard and mouse plug in, became a full desktop.
- Installed update


### RPI desktop Terminals
pg 255
- `$ ls -larth`
   - This command lists the contents of the current directory, or any other
 directory you give it, including listing any hidden files and reporting the sizes of files.
- `$ sudo raspi-config`
   - The sudo part of the command means switch-user do,...


### Using the TTYs
pg 256
- The Terminal application isn’t the only way to use the command-line inter- face: you can also switch to one of a number of already-running terminals known as the teletypes or TTYs. 
Hold the `CTRL` and `ALT` keys on your keyboard and press the `F2` key to switch to `tty2`.
- Using these TTYs is handy when, for whatever reason, the main desktop interface isn’t working.
- To switch away from the TTY, press and hold `CTRL`+`ALT`, then press `F7`: the desktop will reappear.
- Press CTRL+ALT+F2 again and you’ll switch back to tty2 — and anything you were running in it will still be there.

### :star: my summary
try this:
* `$ sudo raspi-config`
* `$ ls -larth`
* `find / -name example.txt`: Searches the whole system for the file example.txt and outputs a list of all directories that contain the file.
* `nano example.txt`: Opens the file example.txt in the Linux text editor Nano.
* `df /`: Shows how much free disk space is available.
* `poweroff`: To shutdown immediately.
* `raspi-config`: Opens the configuration settings menu.
* `reboot`: To reboot immediately.
* `shutdown`-h now: To shutdown immediate
* `free`: Shows how much free memory is available.
* `hostname -I`: Shows the IP address of your Raspberry Pi.

ref: https://www.circuitbasics.com/useful-raspberry-pi-commands/

* To check OS, and version
``` console
pi@raspberrypi:~ $ cat /etc/os-release
PRETTY_NAME="Debian GNU/Linux 12 (bookworm)"
NAME="Debian GNU/Linux"
VERSION_ID="12"
VERSION="12 (bookworm)"
VERSION_CODENAME=bookworm
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
pi@raspberrypi:~ $ ^C
pi@raspberrypi:~ $ cat /proc/version
Linux version 6.12.20+rpt-rpi-v8 (serge@raspberrypi.com) (aarch64-linux-gnu-gcc-12 (Debian 12.2.0-14) 12.2.0, GNU ld (GNU Binutils for Debian) 2.40) #1 SMP PREEMPT Debian 1:6.12.20-1+rpt1~bpo12+1 (2025-03-19)
pi@raspberrypi:~ $ 
```
* to check cpu and board type
   - `$ cat /proc/cpuinfo`
``` console
...
Model		: Raspberry Pi 4 Model B Rev 1.5
```
- to vew real-time view:
   - `$ top`
- to displays total menory:
   - `$ free -h`
   - or `$ cat /proc/meminfo`
   - or use the Menu > Accessories > Task Manager
- TTY
   - hold Ctrl and ALT and press F2
   - to switch off > Ctrl + Alt and F7

---

## Chapter 1 Software installation and Test (necessary)

### Step 1 Obtain the Code and Set python3 as Default

pg42. download the code
``` console
pi@raspberrypi:~ $ cd ~
pi@raspberrypi:~ $ git clone --depth 1 https://github.com/Freenove/Freenove_4WD_Smart_Car_Kit_for_Raspberry_Pi
Cloning into 'Freenove_4WD_Smart_Car_Kit_for_Raspberry_Pi'...
remote: Enumerating objects: 121, done.
remote: Counting objects: 100% (121/121), done.
remote: Compressing objects: 100% (99/99), done.
remote: Total 121 (delta 21), reused 99 (delta 18), pack-reused 0 (from 0)
Receiving objects: 100% (121/121), 182.69 MiB | 6.34 MiB/s, done.
Resolving deltas: 100% (21/21), done.
Updating files: 100% (110/110), done.
pi@raspberrypi:~ $ 
```

- folder Freenove_4WD_Smart_Car_Kit_for_Raspberry_Pi-master, 
rename to Freenove


- check if `python 3`
``` console
pi@raspberrypi:~ $ python
Python 3.11.2 (main, Nov 30 2024, 21:22:50) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

### Step2 configuration
pg45
- RPi > Preference > RaspberryPi Configuration
  - Interfaces > click on SPI, click on I2C
#### command
- `$ lsmod | grep i2c`
``` console
pi@raspberrypi:~ $ lsmod | grep i2c
i2c_bcm2835            16384  0
i2c_brcmstb            12288  0
i2c_dev                16384  0
pi@raspberrypi:~ $ 
```
- `$ sudo apt-get install i2c-tools`
- `$ sudo apt-get install python3-smbus`
- `$ i2cdetect -y 1`

``` console
pi@raspberrypi:~/Freenove $ sudo apt-get install i2c-tools
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
i2c-tools is already the newest version (4.3-2+b3).
The following packages were automatically installed and are no longer required:
  avahi-utils gir1.2-handy-1 gir1.2-packagekitglib-1.0 gir1.2-polkit-1.0
  libc++1-16 libc++abi1-16 libcamera0.3 libunwind-16 libwlroots12
  lxplug-network
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 4 not upgraded.
pi@raspberrypi:~/Freenove $ sudo apt-get install python3-smbus
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
python3-smbus is already the newest version (4.3-2+b3).
python3-smbus set to manually installed.
The following packages were automatically installed and are no longer required:
  avahi-utils gir1.2-handy-1 gir1.2-packagekitglib-1.0 gir1.2-polkit-1.0
  libc++1-16 libc++abi1-16 libcamera0.3 libunwind-16 libwlroots12
  lxplug-network
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 4 not upgraded.
pi@raspberrypi:~/Freenove $ i2cdetect -y 1
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:                         -- -- -- -- -- -- -- -- 
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
70: -- -- -- -- -- -- -- --                         
pi@raspberrypi:~/Freenove $ 
```

pg 47. Additional supplement
``` console
pi@raspberrypi:~ $ cd ~
pi@raspberrypi:~ $ ls /etc/modprobe.d
blacklist-8192cu.conf  raspi-blacklist.conf  rfkill_default.conf
pi@raspberrypi:~ $ 

```
1. Create a new `snd-blacklist.conf` and open it for editing
`sudo nano /etc/modprobe.d/snd-blacklist.conf`
Add following content: After adding the contents, you need to press Ctrl+O, Enter, Ctrl+Z.
```
blacklist snd_bcm2835
```

``` console
pi@raspberrypi:~ $ cd ~
pi@raspberrypi:~ $ ls /etc/modprobe.d
blacklist-8192cu.conf  raspi-blacklist.conf  rfkill_default.conf
pi@raspberrypi:~ $ sudo nano /etc/modprobe.d/snd-blacklist.conf
pi@raspberrypi:~ $ ls -larth /etc/modprobe.d
total 28K
-rw-r--r--   1 root root   17 May 10  2023 blacklist-8192cu.conf
-rw-r--r--   1 root root   31 Oct 30 22:24 rfkill_default.conf
-rw-r--r--   1 root root    0 Apr 13 23:03 raspi-blacklist.conf
drwxr-xr-x 132 root root  12K Apr 13 23:37 ..
-rw-r--r--   1 root root   22 Apr 14 13:24 snd-blacklist.conf
drwxr-xr-x   2 root root 4.0K Apr 14 13:24 .
pi@raspberrypi:~ $ 

```
Edit the config file
- `pi@raspberrypi:~ $ sudo nano /boot/firmware/config.txt`
- Add `#` to comment out the second line. Press Ctrl+O, Enter, Ctrl+X.
``` 
# Enable audio (loads snd_bcm2835)
# dtparam=audio=on
```
Need to restart to take effect

---
### Step 3 Run the Libraries Installation Program
pg 48 `$ sudo python setup.py`
``` console
pi@raspberrypi:~ $ cd ~
pi@raspberrypi:~ $ cd Free*/Code
pi@raspberrypi:~/Freenove/Code $ sudo python setup.py
Hit:1 http://deb.debian.org/debian bookworm InRelease
Hit:2 http://deb.debian.org/debian-security bookworm-secu
...
Enter the camera model (e.g., ov5647 or imx219): ov5647
Updated /boot/firmware/config.txt with 'dtoverlay=ov5647'
Please reboot your Raspberry Pi to complete the installation.
pi@raspberrypi:~/Freenove/Code $ 
```

## Chapter 3 Module test
* S1: Power main switch
* S2: Motor and servo power switch
### Motor
Adjust servor head before assemble
``` console
pi@raspberrypi:~/Freenove/Code/Server $ sudo python servo.py
Now servos will rotate to 90 degree.
If they have already been at 90 degree, nothing will be observed.
Please keep the program running when installing the servos.
After that, you can press ctrl-C to end the program.
^C
End of program
```
#### Wheel test
``` console
pi@raspberrypi:~/Freenove/Code/Server $ sudo python test.py Motor
Program is starting ...
The car is moving forward
The car is going backwards
The car is turning left
The car is turning right

End of program
pi@raspberrypi:~/Freenove/Code/Server $
```
But it travel reversed

Edit motor.py
``` py
    def set_motor_model(self, duty1, duty2, duty3, duty4):
        duty1,duty2,duty3,duty4=self.duty_range(duty1,duty2,duty3,duty4)
        self.left_upper_wheel(-duty1)
        self.left_lower_wheel(-duty2)
        self.right_upper_wheel(-duty3)
        self.right_lower_wheel(-duty4)
```

---
Quick reference
#### login `$ ssh pi@192.168.3.110` and password
#### reboot `$ sudo reboot`
#### shutdown `$ sudo poweroff`

----
