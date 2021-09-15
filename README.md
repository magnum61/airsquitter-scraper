Jetvision Air!Squitter JSON scraper - forked from https://github.com/rmoesbergen/airsquitter-scraper

This program maintains flight data by periodically querying an AirSquitter device

The fields in the CSV file are described here: https://wiki.jetvision.de/wiki/Radarcape:Software_Features

Installation instructions
The program is made to run on a Raspberry PI (or similar mini-computer). Before installing it, the Raspberry must first be provided with an OS + connectivity. Instructions for this can be found here:

https://desertbot.io/blog/headless-raspberry-pi-4-ssh-wifi-setup

Make sure SSH access is possible and the device is accessible via WiFi. Follow the steps below after the PI is accessible on the network:

Log in via SSH on the Raspberry pi
ssh pi@<ip adres pi>
Run the following command: 
curl https://raw.githubusercontent.com/magnum61/airsquitter-scraper/master/install.sh | bash
In /home/pi, modify the file 'airsquitter-settings.json' as desired. See below for explanation of all settings.

Start the program as a service:

sudo systemctl start airsquitter-scraper.service
The program will start in the background and will start generating the log files and CSV. On a reboot of the raspberry pi, the program will automatically be started again.

Settings
All settings are located in a file 'airsquitter-settings.json'. The settings are as follows:

api_url: The location of the AirSquitter aircraflist.json file.
lamin, lamax, lomin, lomax: These 4 parameters specify the geographic area to filter on when retrieving flights.
poll_interval: The interval at which the device is queried, in seconds.
max_geo_altitude: The maximum altitude of a flight for registration in the CSV file, in meters. If the flight is above this limit, it will not be registered in the CSV file.
min_geo_altitude: The minimum altitude of a flight for registration in the CSV file, in meters.
log_file: The location of a (debug) log file in which all responses from the API are logged 1 to 1 unfiltered.
csv_file: The location of the CSV file in which all flights meeting all criteria are written. The file name can contain "date formatting characters" to periodically write a new file. For an overview of formatting characters to use, see: https://strftime.org/ The default example uses "airsquitter-flights-%m.csv", which will write a new file each month, with the month number in the file name, padded with a '0'.
history_file: The location of the file in which the flights that have already been 'seen' are kept. This file is kept to avoid duplicate logging of flights. The format is in JSON and contains all icao24 codes and a timestamp of flights. Upon restarting the program, this history will be read back in, so that flights are never written twice in the CSV file.
keep_history: How long the program 'remembers' the flights in the history file, to deduplicate, in seconds. If a flight is 'seen' 2 or more times within this interval, only 1 record will be written to the CSV file.
min_speed: The minimum speed (in km/h) of a flight. A flight slower than this speed will not be logged.

