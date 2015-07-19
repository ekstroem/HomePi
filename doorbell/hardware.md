# Hardware

I only have very basic knowledge about low voltage electronics so some
of this may or may not be relevant.

## Wiring


As you can see in the picture, my 433 MHz receiver actually has two
Data-pins. Why it has two pins I don’t exactly know, but a possibility
is it is used to easily connect two output-sources. In this project
however we will only need to use one of the pins. I used the one
closest to the GND-pin.

The RPi can provide two voltages 5 V and 3.3 V as can be seen in the
following image (taken from
[here](https://www.raspberrypi.org/documentation/usage/gpio/) ). Note
that I'm using a version 1 RPi model B. The version 2 has 40 pins on
the GPIO.

![GPIO pin layout on RPi B v1.2 ](https://github.com/ekstroem/HomePi/blob/master/doorbell/images/gpio.png) 


I'm not entirely strong on electronics but it seems to me from reading
various blogs and webpages that a
[voltage divider](http://elinux.org/RPi_GPIO_Interface_Circuits) is a
good idea to reduce the voltage from 5 V to 3.3 V. The 433 MHz
receiver expects 5 V but the input pins on the RPi should not served
more than 3.3 V. Therefore we should to reduce the input voltage on
the GPIO from 5 V to 3.3 V.  The construction is seen in the link
above and the two resistors needed are

| 18k | 33k |
|---|---|
| Brown   | Orange |
| Gray    | Orange |
| Orange  | Orange |
| Gold    | Gold   |

I was not aware of this when I threw
the circut together the first time and the 5 V did not fry my RPi. I
tried using the 3.3 V as input to the receiver but then I could not
receive any signals from the door bell.

Specifically I connect pin 13 on the RPi to the 5 VCC on the receiver.



For the antenna I took a 20 cm long wire coiled up around a pencil and
attached it to the receiver.


### 
