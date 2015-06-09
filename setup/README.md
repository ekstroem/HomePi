# Setup for new raspberry pi

(This works at least with the new Raspberry Pi 2)

## Running for the first time

1. Change the default user password from the blue screen (alternatively run `raspi-config` to get it started). The default password is `raspberry`.
2. Change if you want to boot into desktop or command line
3. Change localizations
4. Set overclocking (usually running `medium`)
5. Under advanced options change hostname and enable ssh. Change the memory split if not running a graphical interface. Update the tool

## Update packages in terminal

Fire up a terminal and run the following commands. Might take a some time to complete
```
sudo apt-get update
sudo apt-get upgrade
sudo rpi-update
sudo shutdown -r now
```
After the raspberry starts up again run a terminal and type
```
sudo apt-get dist-upgrade
```

~~I also upgraded to jessie. This might not be relevant when that standard raspbian image includes jessie~~

```
sudo sed -i 's/wheezy/jessie/g' /etc/apt/sources.list
```

~~and then run the update -> upgrade -> dist-upgrade cycle again.  Once you start the actual upgrade you will be given a choice to manually restart any currently running services. Manually restart services is recommended as this give you an option to restart services selectively eg. SSH etc. Furthermore, if you are performing the update via SSH and have no physical access to your Raspberry PI make sure not to disable SSH root access.~~

```
Disable SSH password authentication for root?  NO
```



## Enable WiFi


