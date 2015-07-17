# Door bell


## Hardware

Got the wiring to work. See the following image.


## Install the python libraries

Install and/or update the relevant python libraries and the GPIO library
```
sudo apt-get update
sudo apt-get install python-dev python-pip
sudo pip install --upgrade distribute
sudo pip install ipython

sudo pip install --upgrade RPi.GPIO
```

## Test to see if we get something

```python
#!/usr/bin/python
# Hello world python
print "Gestart"
import RPi.GPIO as GPIO

GPIO.setmode(GPIO.BOARD);
GPIO.setup(21, GPIO.IN)
while True:
    input_value = GPIO.input(21)
    print "--" + str(input_value)
```
