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
various books, blogs and webpages that a
[voltage divider](http://elinux.org/RPi_GPIO_Interface_Circuits) that
reduces the voltage from 5 V to 3.3 V is necessary to prevent the risk
of frying the RPi. We need to do reduce the input voltage on the GPIO
from 5 V to 3.3 V because the 433 MHz receiver expects 5 V but the
input pins on the RPi should not served more than 3.3 V.  The
construction is seen in the link above and the two resistors I used
were 

| 18k | 33k |
|---|---|
| Brown   | Orange |
| Gray    | Orange |
| Orange  | Orange |
| Gold    | Gold   |

Any two resistors where one has roughly have the resistance of the
other would work.

I was not aware of the need for a voltage divider when I threw the
circut together the first time and the 5 V did luckily not fry my RPi
but I'm not taking any chances so I added the voltage divider. To be
honest I throught that since the RPi GPIO can provide 5 V by itself that it
also could take 5 V. Apparently not.  I tried using the 3.3 V as input to
the receiver but then I could not receive any signals from the door
bell, so I'm not sure if the 433 receiver works correctly when it is
underpowered (maybe not a big surprise there).

Specifically I connect pin 13 on the RPi to the 5 VCC on the
receiver. I put everything on a breadboard to make it somewhat
compact. A (slightly crappy) image is shown below.

![Final version](https://github.com/ekstroem/HomePi/blob/master/doorbell/images/final.jpg) 

## Antenna

For the antenna I took a 50 cm long wire coiled up around a pencil and
attached it to the receiver. There are probably something clever you
could do with computing the ideal length, but I didn't. However I
found out that it I run the RPi with a power supply wiht 1 amp then I
had trouble receiving the signal. If I use a power supply with 2 amps
then things are fine and dandy.

However, I recently read [here](http://www.antenna-theory.com/)
and [here](http://forum.arduino.cc/index.php?topic=297815.0) that one of the optimal lengths for a 433 MHz antenna
should be 17.2 cm.
