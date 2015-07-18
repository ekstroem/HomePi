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


```cpp



```


## Creating the Python script

Create the following python script. I named the file `tweet.py` and
made sure it was executable. The keys are *not* the correct ones (in
case you were wondering).

```python
#!/usr/bin/env python
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
    print "Tweeted: " + tweetStr
```


