# setting-up-an-rtc-module
This repository deals on showing you how to add the DS3231 real-time clock (RTC) module to your Raspberry Pi.

#STEP-1,connecting th RTC
for connection diagram check the attached image(images.jpg)
    
    Vin connects to Pin 1
    SDA connects to Pin 3
    SCL connects to Pin 5
    GND connects to Pin 6

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

![image](https://github.com/user-attachments/assets/8eb8b9cc-7f5c-4e4d-98c4-5ce279a97a83)

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

