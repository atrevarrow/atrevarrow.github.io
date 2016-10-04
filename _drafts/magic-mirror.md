---
layout: post
title:  "DRAFT - Building a magic mirror"
categories: ["raspberrypi", "diy", "hardware"]
---
Install an OS on the Pi - I just used NOOBS to setup Raspbian

Boots to GUI - I had to connect to wifi manually at this point. Change hostname (magicmirror), timezone and password.

Open terminal and install helper tools: sudo apt-get install curl wget git build-essential unzip

mkdir MagicMirror, cd MagicMirror
git clone https://github.com/atrevarrow/MagicMirror.git .
(I had already forked the original repo which is at: https://github.com/MichMich/MagicMirror)

Initial setup - manually going through the steps in installers/raspberry.sh
Install nodejs
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs

Setup MagicMirror: npm install

Run it: npm start
(Should get the "Please create a config file" message displayed)


To setup and run locally on my desktop:
- clone the repo
- remove the prepublish snyk protect script from package.json: "prepublish": "npm run snyk-protect"
- npm install, npm start


Done:
- setup required modules
- metservice rain radar?
- created module to simulate user presence/absence

Todo:
- stop display auto shut-off
- rotate display
- run MM at startup
- fitbit module
- metservice current weather/forecast?
- use PIR sensor to auto switch-on/off display
