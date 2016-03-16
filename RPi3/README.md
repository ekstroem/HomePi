# RPi 3 Setup

Note about getting a RPi 3 version up and running.

## Preparation

1. Download the jessie-lite image and write it to a SD card


## Running the RPi3 for the first time

Add a screen a keyboard to the Pi when running for the first time. I
also included an RJ45 cable for easier update access.

1. Login using user `pi` and password `raspberry`
2. Run `sudo raspi-config`
    1. Expand the file system
    2. Change the password
	3. Enable SSH access under the advanced tab
	4. Possibly set the hostname
3. Reboot.


## Updating file system

Login and run

1. sudo apt-get update
2. sudo apt-get upgrade

