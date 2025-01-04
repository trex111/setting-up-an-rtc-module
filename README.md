# setting-up-an-rtc-module
This repository deals on showing you how to add the DS3231 real-time clock (RTC) module to your Raspberry Pi.

#STEP-1,connecting th RTC
for connection diagram check the attached image(images.jpg)
    
    Vin connects to Pin 1
    SDA connects to Pin 3
    SCL connects to Pin 5
    GND connects to Pin 6
![image](https://github.com/user-attachments/assets/fbc43b05-8bba-4e58-ac7e-6d04b3c5f97c)

if its  modele like the picture itshouldjust slide into it by gentle push (refer images.jpg)

![image](https://github.com/user-attachments/assets/27864b2c-04f4-4e86-8c23-583605903519)

#STEP-2,Enabling the I2C on the raspberry pi 
1.We need to enable I2C in theraspberry pi.for that we need to mke changes in the config.txt file.type the following commands
to edit the file

    sudo nano /boot/config.txt

The above command opens the config.tx file
2.Add the below two lines.

    dtparam=i2c_arm=on (this line might becommented out like #dtparam=i2c_arm=on)
    dtoverlay=i2c-rtc,ds3231

3.Now save the config.txt file by pressing ctrl+x and then y to save changes and exit
4.Now reboot your pi 

    sudo reboot

#step-3,Configuring the Raspberry pi
1.Let’s begin this tutorial by ensuring our Raspberry Pi is entirely up to date; this ensures that we will be utilizing all the latest software available.
open terminal and execute the following commands:-

    sudo apt update
    sudo apt upgrade

2.With the Raspberry Pi now entirely up to date we can now run its configuration tool to begin the process of switching on I2C.
Run the following command to launch the configuration tool.

    sudo raspi-config

3.This command will bring up the configuration tool; this tool is an easy way to make a variety of changes to your Raspberry Pi’s configuration. Today, however, we will only by exploring how to enable the I2C interface.
Use the ARROW KEYS to go down and select “3 Interface Options“. Once this option has been selected, you can press ENTER.

4.On the next screen, you will want to use the ARROW KEYS to select “I2C“, press ENTER once highlighted to choose this option.

5.You will now be asked if you want to enable the “ARM I2C Interface“, select “<Yes>” with your ARROW KEYS and press ENTER to proceed.

6.Once the raspi-config tool makes the needed changes, the following text should appear on the screen: “The ARM I2C interface is enabled“.

However, before I2C is genuinely enabled, we must first restart the Raspberry Pi. To do this first get back to the terminal by pressing ENTER and then ESC.

Type the following command into the terminal on your Raspberry Pi to restart it.

    sudo reboot
  
7.Once the Raspberry Pi has finished restarting we need to install an additional two packages, these packages will help us tell whether we have set up I2C successfully and that it is working as intended.

Run the following command on your Raspberry Pi to install python-smbus and i2c-tools:

    sudo apt-get install i2c-tools

8.With those tools now installed run the following command on your Raspberry Pi to detect that you have correctly wired up your RTC device.

    sudo i2cdetect -y 1

![image](https://github.com/user-attachments/assets/8eb8b9cc-7f5c-4e4d-98c4-5ce279a97a83)

You should get an output like the above image.

The 0x68 is a popular address for real-time clocks, especially for the DS3231. Having a 68(instead of the UU) on the address means that a driver wasn’t using the address. If the address result is “UU”, it is currently being used by a driver.
#STEP-4,Use the DS3231 as the main clock
1.To use the DS3231 as the main clock we nee to edit the 'hwclock-set' file 
Run the following command in the  terminal and comment out the lines given below

        sudo nano /lib/udev/hwclock-set

2.comment out the lines given below:

        if [ -e /run/systemd/system ] ; then
        exit 0
        fi

3.After change(commenting out(#)) it should look like this

        #if [ -e /run/systemd/system ] ; then
        # exit 0
        #fi

4.Now save the file by pressing ctrl+x and then y to save changes and exit

5.Now reboot your pi 

    sudo reboot

6. Once your Raspberry Pi has finished restarting we can now run the following command, this is so we can make sure that the kernel drivers for the RTC Chip are loaded in.

        sudo i2cdetect -y 1

7.You should see a wall of text appear, if “UU” appears instead of “68” then we have successfully loaded in the Kernel driver for our RTC circuit.

8.Now that we have successfully got the kernel driver activated for the RTC Chip and we know it’s communicating with the Raspberry Pi, we need to remove the “fake-hwclock“ package. This package acts as a placeholder for the real hardware clock when you don’t have one.

Type the following two commands into the terminal on your Raspberry Pi to remove the fake-hwclock package. We also remove hwclock from any startup scripts as we will no longer need this.

        sudo apt -y remove fake-hwclock
        sudo update-rc.d -f fake-hwclock remove

#step-5,Syncing time from the Pi to the RTC module
Now that we have our RTC module all hooked up and Raspbian and the Raspberry Pi configured correctly we need to synchronize the time with our RTC Module. The reason for this is that the time provided by a new RTC module will be incorrect.

1.You can read the time directly from the RTC module by running the following command if you try it now you will notice it is currently way off our current real-time.

        sudo hwclock -D -r

2.Now before we go ahead and sync the correct time from our Raspberry Pi to our RTC module, we need to run the following command to make sure the time on the Raspberry Pi is in fact correct.If the time is not right, make sure that you are connected to a Wi-Fi or Ethernet connection.

        date
        
3. If the time displayed by the date command is correct, we can go ahead and run the following command on your Raspberry Pi.This command will write the time from the Raspberry Pi to the RTC Module.

        sudo hwclock -w

4.Now if you read the time directly from the RTC module again, you will notice that it has been changed to the same time as what your Raspberry Pi was set at.

        sudo hwclock -r

#You should hopefully now have a fully operational RTC module that is actively keeping your Raspberry Pi’s time correct even when it loses power or loses an internet connection. I hope you have enjoyed this fun Pi project and will put it to good use.
