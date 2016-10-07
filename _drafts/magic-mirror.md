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

Edit config: setup correct keys/ids/calendar urls

Get MMM-PIR-Sensor: follow installation instructions at https://github.com/paviro/MMM-PIR-Sensor
Add MMM-PIR-Sensor to MM config, and setup correct sensorPIN setting (default 22)

Get MMM-MetServiceRadar: in modules, git clone https://atrevarrow@bitbucket.org/atrevarrow/mmm-metserviceradar.git

Had to disable overscan (in /boot/config.txt) to ensure full display was used: https://www.raspberrypi.org/forums/viewtopic.php?f=46&t=47152

Auto start MM at boot: https://github.com/MichMich/MagicMirror/wiki/Auto-Starting-MagicMirror
- sudo npm install -g pm2
- pm2 startup (shows you the command you need to enter to setup)
- Create MM start script (mm.sh) and chmod +x mm.sh



Fitbit: follow setup instructions at https://github.com/SVendittelli/MMM-fitbit:
- register app at dev.fitbit.com
- clone MMM-fitbit module into modules directory
- Had to install python (3.5.2) on my dev machine before the dependencies below
- Install dependencies - cd modules/MMM-fitbit then:
    - npm install python-shell
    - sudo pip install -r python/fitbit/requirements.txt
- add app client_id/client_secret to credentials.ini

- had python errors, so removed Python 3.5.2, installed 2.7.12


Squeezebox player:
- Install squeezelite as a service) as per this page:
  http://www.winko-erades.nl/index.php?option=com_content&view=article&id=54:installing-squeezelite-player-on-a-raspberry-pi-running-jessie&catid=20:raspbian
    - sudo apt-get install squeezelite
    - sudo apt-get install libflac-dev



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
- merge regular and birthday calendars into one?

