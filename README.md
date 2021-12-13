# ESPEASY_Plugin_ITHO

**Note: this repository is deprecated and will not be updated. The Itho plugin moved from the plugin playground to the ESPEasy main repository (https://github.com/letscontrolit/ESPEasy).
It will be available in the testing D build of the mega as plugin P118. Mega_20211105 is the latest version which does not include it, and is therefore available in this repository (still as P145).
You will have to set all settings again after moving from P145 to P118, it is not required to re-join the ESP with the Itho fan if the same device ID is used.**

Plugin for ESPEasy regarding a ITHO Fan remote

A CC1101 868Mhz transmitter is needed (available on ebay or aliexpress)

This plugin is using the **NEW** library from: https://github.com/arjenhiemstra/IthoEcoFanRFT/
made by 'arjenhiemstra' based on the library made by 'supersjimmie' and 'klusjesman'.
It is now possible to program the Device ID of your ESPEasy in case you have a neighbour who also runs a node.

The original ESPEasy Itho plugin was made by jodur: https://github.com/jodur/ESPEASY_Plugin_ITHO

For more info see also: https://gathering.tweakers.net/forum/list_messages/1690945

And for a manual (in Dutch): https://docs.google.com/document/d/e/2PACX-1vRoO1TfkUKkyQlgzsmm7XzI3xyUXMBMrqwF6NJXVwQmykP6gMtsJWp-x2vYiHDEwyScqhsPkJHGGYs-/pub

**Latest release: ESP_Easy_mega_20211105_normal_ESP8266_4M1M-newlib.bin**

|CC11xx pin    |ESP pins|Description                                        |
|:-------------|:-------|:--------------------------------------------------|
|1 - VCC       |VCC     |3v3                                                |
|2 - GND       |GND     |Ground                                             |
|3 - MOSI      |13=D7   |Data input to CC11xx                               |  
|4 - SCK       |14=D5   |Clock pin                                          |
|5 - MISO/GDO1 |12=D6   |Data output from CC11xx / serial clock from CC11xx |
|6 - GDO2      |04=D1*  |Interrupt pin                                      |
|7 - GDO0      |NC      |Not in use                                         |
| 8 - CSN      |15=D8   |Chip select / (SPI_SS)                             |

*Note: GDO2 is used as interrupt pin for receiving and is configurable in the plugin

Not recommended pins for intterupt:
- Boot related pins D3(GPIO0) and D4 (GPIO2) 
- Pin with no interrupt support: D0 (GPIO16)
- If connected to the default I2C pins make sure to disable I2C in the ESPEasy hardware settings

## List of commands:

1111 to join ESP8266 with Itho ventilation unit

9999 to leaveESP8266 with Itho ventilation unit

0 - set Itho ventilation unit to standby

1 - set Itho ventilation unit to low speed

2 - set Itho ventilation unit to medium speed

3 - set Itho ventilation unit to high speed

4 - set Itho ventilation unit to full speed

13 - set itho to high speed with hardware timer (10 min)

23 - set itho to high speed with hardware timer (20 min)

33 - set itho to high speed with hardware timer (30 min)


## List of States:

1 - Itho ventilation unit to lowest speed

2 - Itho ventilation unit to medium speed

3 - Itho ventilation unit to high speed

4 - Itho ventilation unit to full speed

13 -Itho to high speed with hardware timer (10 min)

23 -Itho to high speed with hardware timer (20 min)

33 -Itho to high speed with hardware timer (30 min)

In the plugin you are able to define 3 RF device ID's for the existing RF remote controls the plugin is listening to, to update the state of the fan.
You are able to capture the id of you RF remote, by setting the log settings to 3, in the advanced settings menu. After pressing a button, you will see the ID (23 chars) of the RF in the log. Use ID with ':'. 
#### example ID: 11:22:33:44:55:66:77:88

In case a timerfunction is called (timer 1..3), an internal timer is running as estimate for the elapsed time.

The plugin will publish MQTT topics as they change. The aquisition cycle time should be used as a state update cycle time.
In case a topic doesnT change the cycle time is used for cyclic update. It is recommended to set this to higher values: for example to 60s

For the lazy people or people with no codeskills a binary is added for ESPEASY with all stable plugins including this plugin

When using a Wemos D1 mini, you have to remove D1 and D2 from I2C on the hardware page, because one of the pins must be used as Intterupt pin. See note above concerning the interrupt pins.

### Paring the ESP8266 remote with the fan

To control the fan, the ESP8266 has to be "paired" with the fan. To pair the ESP8266 with the fan, you first have to powerdown (pull the plug) of the itho fan for minimal of 15 sec. After restoring the power you the are able to pair the ESP8266 within 2 min after restoring the power

### Connect/Join with https-command:
http://YourIP-adress/control?cmd=STATE,1111

### Connect/Join with MQTT-command form mosquito command line
mosquitto_pub -t /Fan/state/cmd -m 'state 1111'
