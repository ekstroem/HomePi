# Getting the Raspberry to tweet

Most of this was taken/adapted from
[here](http://www.makeuseof.com/tag/how-to-build-a-raspberry-pi-twitter-bot/)

http://www.instructables.com/id/Raspberry-Pi-Twitterbot/step5/Create-Twitter-app/


## Installing Twython

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install python-setuptools
sudo easy_install pip
sudo pip install twython
```


## Registering a Twitter app

What an enourmous bunch of crap I had to get through to get that app
registered. Apparently my phone carrier is not recognized by twitter,
so had to get their support to make an exception.

## Python script

```python
#!/usr/bin/env python
import sys
from twython import Twython

tweetStr = "RIP Peeraphan Palusuk, 68, Thai politician, Minister of
Science and Technology (since 2013), MP for Yasothon (since 1985)"

# your twitter consumer and access information goes here
# note: these are garbage strings and won't work
apiKey = 'roasdkoqwkk8i10kwks09aka'
apiSecret = '4ghmkjal810kdla0wkkoasi'
accessToken = '1239821-dakos81koamow9918ma0sadsqq'
accessTokenSecret = 'saklasooqjdoajfj8f9981mska01mdka09'

api = Twython(apiKey,apiSecret,accessToken,accessTokenSecret)

api.update_status(status=tweetStr)

print "Tweeted: " + tweetStr

```

