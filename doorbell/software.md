# Software

I originally wanted to control everything using Python btu I couldn't
get the reading/decoding of the signal to work. Admittedly, I did not
try very hard since I could tweak the C code that was part of the 433
utils, so the general construction is based on two programs:

1. A C program that continuously monitors the receiver and reacts on
   the signals and code received. If the code from the door bell is
   received then it runs a python script:
2. The python script accepts a string on the command line and tweets
that string to a specific account.


## Initial software installation

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

In the `433Utils/RPi_utils` is the RFSniffer program (and code) that
we will tweak to our use.


## Hacking the C code

This is a hacked version of the `RFSniffer` program that comes with
the 433Utils installed above. Basically I took the the `RFSniffer.cpp`
source code and used it as a basis for the  code.  The following code
is saved as `doorbell.cpp`.

See the note below though!

```cpp
/*
  doorbell
  
  Hacked from http://code.google.com/p/rc-switch/  
  by @justy to provide a handy RF code sniffer

  Further Hacked by Claus Ekstrøm
  https://github.com/ekstroem/HomePi
  
*/

#include "RCSwitch.h"
#include <stdlib.h>
#include <stdio.h>
#include <time.h>     
     
RCSwitch mySwitch;
 
int main(int argc, char *argv[]) {

  // This gets the current time
  time_t newtime, lasttime;
  struct tm * timestruct = localtime(&newtime);
  char timestring[30];
  int waitingtime = 0;
  int secondsbetweenrings = 10;
  
  // This is the bell code for my bell. You should change this to match your own door bell
  unsigned long bellcode = 13066068;  
  char buf[128];

  lasttime = time(0);
  newtime = lasttime;
  timestruct = localtime(&newtime);
  strftime (timestring, 30, "%c" ,timestruct);

  printf ( "%s --- Starting listener\n",  timestring);

  // This pin is not the first pin on the RPi GPIO header!
  // Consult https://projects.drogon.net/raspberry-pi/wiringpi/pins/
  // for more information.
  int PIN = 2;
  
  if(wiringPiSetup() == -1)
    return 0;
  
  mySwitch = RCSwitch();
  mySwitch.enableReceive(PIN);  // Receiver on inerrupt 0 => that is pin #2
  
  unsigned long value;
  
  while(1) {
    
    if (mySwitch.available()) {
      
      value = mySwitch.getReceivedValue();
      
      if (value == 0) {
	printf("Unknown encoding\n");
      } else {    
	// This is where the exciting things happen
	
	// Figure out what time it is and compute the number of seconds since last ring
	// Can use that to give a minimum time between rings to prevent events from "ghosting" or noise
	newtime = time(0);
	timestruct = localtime(&newtime);
	waitingtime = (int) difftime(newtime, lasttime);

	strftime (timestring, 30, "%c" ,timestruct);

	// Print a registration for a log file   
	printf("%s --- Received %lu\n", timestring,  mySwitch.getReceivedValue());

	// Check that the code is indeed from the doorbell. This should be changed depending on the actual door bell signal
	if (mySwitch.getReceivedValue() == bellcode) {
	  lasttime = newtime; 
	
	  if (waitingtime>secondsbetweenrings) {	    
	    // printf("  Now I want to tweet!\n");
	    strftime (timestring, 30, "%c" ,timestruct);
	    snprintf(buf, sizeof(buf), "su - pi -c 'python3 /home/pi/bin/tweet.py Ding Dong! Door bell rang on %s'", timestring);
	    // Now this is not that secure since we have no control over tweet.py
	    system(buf);
	  }
	}	
      }      
      mySwitch.resetAvailable();      
    }  
  }
  exit(0);
}
```

This can be compiled directly or by adding the following two lines to
  the `Makefile` in the `RPi__utils` directory. 

```
doorbell: RCSwitch.o doorbell.o
	$(CXX) $(CXXFLAGS) $(LDFLAGS) $+ -o $@ -lwiringPi
```

Then run `make` and you should have a `doorbell` program that you can use.

### Interrupt-based triggering

The code above works fine but is a cpu-hog, and it would be better to
write the program around an interrupt such that it is triggered when a
signal is received instead of polling the GPIO several times per
second.

I have found a set of web pages that describes how to do this in
python, and possibly it could be set up using the `wiringPiISR` in the
C++ code listed above. I havenøt found a final solution to this yet.

* http://raspi.tv/2013/how-to-use-interrupts-with-python-on-the-raspberry-pi-and-rpi-gpio
* http://www.themagpi.com/issue/issue-7/article/interrupts-and-other-activities-with-gpio-pins/
* http://wiringpi.com/reference/priority-interrupts-and-threads/
* https://www.raspberrypi.org/forums/viewtopic.php?f=44&t=7509&start=25



## Creating the Python script

I wanted to run the script using python3 which gave a few
problems. First the twython module should be installed for python3.

```
sudo apt-get install python3-setuptools
sudo easy_install3 pip
```
This gives us access to `pip3.2` that is used to install modules for
python3.

```
sudo pip3.2 install twython
```

Create the following python script. I named the file `tweet.py` and
made sure it was executable. The keys are *not* the correct ones (in
case you were wondering).

```python
#!/usr/bin/env python3
import sys
from twython import Twython

tweetStr = ' '.join(sys.argv[1:])

# your twitter consumer and access information goes here
# note: these are garbage strings and won't work
apiKey = 'ahsdfkljhasdf'
apiSecret = 'kjahsdfpiuhawefgiuhaserg'
accessToken = 'asdkfjnvæirnæakjdsfhæure'
accessTokenSecret = 'æansæinasæjdkfnlkjasdhfluefjlasdfjh'

api = Twython(apiKey,apiSecret,accessToken,accessTokenSecret)

if len(tweetStr)>0 :
    api.update_status(status=tweetStr)
    print ("Tweeted: " + tweetStr)
```


