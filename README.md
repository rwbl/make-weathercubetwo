# make_project_waethercubeone
Display weather information on an LCD display  build in a cube.

![weathercubeone2](https://user-images.githubusercontent.com/47274144/52946335-169b2500-3374-11e9-819a-2589ca2f38a6.png)

# Introduction
WeatherCubeOne2 is the next development level based on weathercubeone1.
Completely rewritten using MQTT. MQTT is a great Technology - the new solution is much simpler then the previous - No need for complex JavaObjects / Inline Java / httpjobs as subscribe and publish messages handles the flow of data.
It also enables to build UI clients, e.g. for B4J and B4A with almost the same code.
The TinkerForge MQTT topics published, are converted and displayed on the TinkerForge LCD20x4 display and published to the Domoticz Server.

For more information, read the detailed description, the readme.txt or the project source code.
The source code is well documented with hints on how to install MQTT and use with B4J, Lazarus or any other MQTT clients.

## Functionality
* Display Date & Time, Air Pressure, Temperature and Humidity on a TinkerForge LCD 20x4 display and on a Domoticz Client Browser.
* Display Trend (Arrow Up / Down) via Custom Characters
* The LCD 20x4 Display buttons function as 1 = Backlight On; 2 = Backlight Off; 3 = Show WeatherCubeOne Version; 4 = Shutdown the WeatherCubeOne.
* To obtain information send from the TinkerForge Bricklets subscribe to the Topics: weathercubeone/airpressure, weathercubeone/timestamp, weathercubeone/temperature or weathercubeone/humidity.
* To trigger an action on the WeatherCubeOne, following topics can be published: weathercubeone/backlighton, weathercubeone/backlightoff, weathercubeone/clear, weathercubeone/copyright, weathercubeone/close, weathercubeone/shutdown.
* Enables clients like B4J UI (example included), B4A (not ready), Lazarus UI (example included)
* But also any other MQTT client (Windows, Android, Apple) or own developments
* Improved Raspberry Pi memory usage
* Example Google Line Charts Temperature, Airpressure, Humidity updated via B4J server using MQTT
* Using TinkerForge Brick MQTT Proxy, B4X MQTT Client Library, Domoticz MQTT

![weathercubeone2processes](https://user-images.githubusercontent.com/47274144/52946342-1733bb80-3374-11e9-8024-c1976c2f6b6e.png)
![weathercubeone2b4jgooglechart](https://user-images.githubusercontent.com/47274144/52946339-169b2500-3374-11e9-8309-a5fb127e4cfb.png)

## Hardware
* Raspberry Pi 2 Model B v1.1
* TinkerForge Master Brick (connected to the Raspberry Pi), Bricklets Temperature, Barometer, Humidity and LCD20x4

## Software
* TinkerForge Brick MQTT Proxy
* B4X jMQTT Client Library
* Domoticz MQTT
* Domoticz Home Automation Server
* Software B4J Application WeatherCubeOne2 & WeatherCubeOneGoogleChart

## MQTT Components
* TinkerForge Brick MQTT Proxy
* B4X jMQTT Client Library
* Domoticz MQTT Broker

![weathercubeone2b4jclient](https://user-images.githubusercontent.com/47274144/52946337-169b2500-3374-11e9-81ab-63e612bc0ca4.png)
![weathercubeone2b4jmqttclient](https://user-images.githubusercontent.com/47274144/52946340-1733bb80-3374-11e9-990f-c399930004a3.png)
![weathercubeone2lazclient](https://user-images.githubusercontent.com/47274144/52946341-1733bb80-3374-11e9-873b-1bb33d18456c.png)

## Additional Notes for the WeatherCubeOne

### MQTT 
As MQTT is used, it is possible to subscribe or publish to the TinkerForge Master Brick or Bricklets or Domoticz topics. 
The payload has to be parsed with JSON. See the examples B4J & Lazarus MQTT client.
To obtain information send from the TinkerForge Bricklets subscribe to the Topics: weathercubeone/airpressure, weathercubeone/timestamp, weathercubeone/temperature or weathercubeone/humidity.
To trigger an action on the WeatherCubeOne, following Topics can be published: weathercubeone/backlighton, weathercubeone/backlightoff, weathercubeone/clear, weathercubeone/copyright, weathercubeone/close, weathercubeone/shutdown.
Any MQTT can be used to do so.
To note is that there are additional topics possible to direct subscribe or publish to the TinkerForge Master Brick or Bricklets. 
The same applies for Domoticz. To use these JSON is required. To read more see the references listed below.

### Autostart TinkerForge MQTT Broker and B4J weathercubeone jar application
Autostart on the Raspberry Pi the TinkerForge MQTT Broker and B4J weathercubeone jar application
Add to /etc/rc.local:
```
  #TinkerForge Brick MQTT Proxy
  cd /home/pi/tinkerforge/mqtt 
  #
  #python brick-mqtt-proxy.py&
  #Default interval is 3 seconds, change to 1 min by setting to 60
  python brick-mqtt-proxy.py --update-interval 60 &
  printf "TinkerForge Brick MQTT Proxy started"

  # WeatherCubeOne
  cd /home/pi/weathercubeone
  /usr/bin/java -jar weathercubeone.jar &

  exit 0
```

### Domoticz Switch showing Copyright
#### Create an executable script
login as: pi
cd /home/pi/weathercubeone
sudo nano script_copyright
Content of the Script:
	#!/bin/bash
	# Script to show copyright message on weathercubeone
	echo "WeatherCubeOne show copyright..."
	mosquitto_pub -h localhost -t weathercubeone/copyright -m ""
Make the Script executable.
sudo chmod 755 script_copyright

#### Create a Switch in Domoticz
Create a new Virtual Switch
Edit and define as action:
script:///home/pi/weathercubeone/script_copyright

#### Subscribe to all Topics
login as: pi
$mosquitto_sub -h weathercubeone-ip-address -t weathercubeone/#

## Soure Code
The source code and required libraries can be found in archive __weathercubeone2.zip__. The code is well documented.
