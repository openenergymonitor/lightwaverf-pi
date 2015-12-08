lightwaverf-pi MQTT
==============

Send OOK RF messages to control Lightwave RF devices using MQTT. Customised for the emonPi to set OOK RF Tx on GPIO 18 and MQTT localhost mosquitto server. 

For more details see [OOK section of emonPi Wiki](http://wiki.openenergymonitor.org/index.php?title=EmonPi#LightWaveRF_OOK ). 

This code is a port of the excellent [lightwaverf arduino library](https://github.com/lawrie/LightwaveRF) written by Lawrie Griffiths to the Raspberry Pi. It utilises the [WiringPi](http://wiringpi.com/) library to do most of the GPIO work. 


# To Install LWRF MQTT on Raspberry Pi 

	sudo apt-get update
  	sudo apt-get install wiringpi
  	sudo apt-get install mosquitto-client
	git clone http://git.eclipse.org/gitroot/paho/org.eclipse.paho.mqtt.c.git
	cd org.eclipse.paho.mqtt.c
  	make
  	sudo make install

  	git clone https://github.com/openenergymonitor/lightwaverf-pi
  	cd lightwaverf-pi
	make
  	sudo make install
  	sudo service lwrfd start

To view log:

  	tail /var/log/daemon.log -f


To make service run at startup:

  	sudo update-rc.d lwrfd defaults

Once installed lwrf plugs can be controlled by publishing to 'lwrf' MQTT topic.

The message format is “<channel> <command> [<level>]”, where channel is 1-16, command is 0-2 (2 means mood), and level is 0-31 (used for dimmers).

E.g publishing “1 1” to 'lwrf' topic turns channel 1 on and “1 0” turns it off.

Plugs can be paired with the emonPi in the usual LightwaveRF way: Either press and hold pairing button (if button exists) or turn on the plug from main power and send 'on' command. Most LightWaveRF plugs allow multiple (up to 6) control devices to be paired.

To reset the plug and delete all pairing press and hold the pairing button to enter pairing mode then press and hold again to erase memory then press (don't hold) once to confirm. For plugs without a pairing button turn on the plug from the mains power then in the first few seconds press the 'all off' button on the RF remote.
