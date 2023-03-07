# Raspberry Pi

- [Install](#install)
- [Connect via SSH](#connect-via-ssh)
- [GPIO](#gpio---general-purpose-input-output)

## Install

- Download [Raspberry Pi Imager](https://www.raspberrypi.com/software/)
- Choose an OS
- Select a storage (micro SD card must be inserted)
- Write the image on the micro SD
- Insert the SD card into the Raspberry Pi
- Power it up and follow the install process

## Connect via SSH

**SSH** is installed by default on the **RPi**.  

- [Connect locally](#connect-locally)
- [Connect remotely](#connect-remotely)

### Connect locally

- Connect to the same Wifi network and using an SSH client enter the following:  
`ssh <userName>@raspberrypi` => e.g: `ssh matt@raspebrrypi`  
- Enter the password when asked

### Connect remotely

## GPIO - General Purpose Input Output  

`import RPi.GPIO as GPIO`

RPi.GPIO  
This is a Python module to control the GPIO on a Raspberry Pi. It includes basic output function and input  
function of GPIO, and functions used to generate PWM.  
GPIO.setmode(mode)  
Sets the mode for pin serial number of GPIO.  
mode=GPIO.BOARD, which represents the GPIO pin serial number based on physical location of RPi.  
mode=GPIO.BCM, which represents the pin serial number based on CPU of BCM chip.  
GPIO.setup(pin,mode)  
Sets pin to input mode or output mode, “pin” for the GPIO pin, “mode” for INPUT or OUTPUT.  
GPIO.output(pin,mode)  
Sets pin to output mode, “pin” for the GPIO pin, “mode” for HIGH (high level) or LOW (low level).  

## Buttons and LEDs

Input:  
buttons, switches, sensors and etc.  
Control:  
RPI, Arduino, MCU and etc.  
Output:  
LED, buzzer, motor and etc.

## Button Event

GPIO.add_event_detect(channel, GPIO.RISING, callback=my_callback, bouncetime=200)  
This is an event detection function. The first parameter specifies the IO port to be detected. The second
parameter specifies the action to be detected. The third parameter specifies a function name; the function
will be executed when the specified action is detected. The fourth parameter is used to set the jitter time.