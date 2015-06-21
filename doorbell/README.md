# General idea

* Use a 433 MHz transmitter to send a wireless signal to the raspberry when the door bell button is pushed
* Use the RPi to make a notification

My current door bell is a wireless one that already uses a 433 MHz
signal so there is no need to rig up a transmitter. I just need to
capture the signal that it sends out.

Most of the information here was copied verbatim from
[this page](http://www.princetronics.com/how-to-read-433-mhz-codes-w-raspberry-pi-433-mhz-receiver/)
or from [this YouTube clip](https://www.youtube.com/watch?v=RHJVyMYJ1XQ).

## Requirements

* Raspberry Pi
* A 433 MHz receiver
* A door bell. I used this one
  ![alt text](https://github.com/ekstroem/HomePi/doorbell/images/doorbell.jpg
  "My door bell")
  


## Hardware installation


As you can see in the picture, my 433 MHz receiver actually has two
Data-pins. Why it has two pins I don’t exactly know, but a possibility
is it is used to easily connect two output-sources, or two different
Arduinos. In this project however we will only need to use one of the
pins. I used the one closest to the GND-pin.


## Immediate to do

* Use the wiring/arduino modules This means that I should
    * check the coltage on our regular door bell.
    * Hook up the transmitter module so that it sends a signal when the button is pushed
    * Check that it works with the arduino
    * Rig up the receiver module to workd with the GPIO on the RPi.
    * Code the software

## Software

WiringPi is a library used to control GPIO in C and much more. It is
installed with the following commands

```
git clone git://git.drogon.net/wiringPi
cd wiringPi
./build
```
The installation produces the following note
```
NOTE: To compile programs with wiringPi, you need to add:
-lwiringPi
to your compile line(s) To use the Gertboard, MaxDetect, etc.
code (the devLib), you need to also add:
-lwiringPiDev
to your compile line(s).
```


In addition we need the following library to interface with the
receiver. This library uses the wiringPi lib from above so we need to
compile that first.

```
git clone git://github.com/ninjablocks/433Utils.git
cd 433Utils/RPi_utils
make
```


## Potential problems?

* Will the be too much triggering noise from other products on the
  receiver end?
