# Smart-Doorbell
Smart doorbell that can be implemented in any existing doorbell based upon a NodeMCU, a 3v3 Relay and Tasmota

# Table of Contents
1. [Materials needed](#materials)
2. [Connecting the hardware](#hardware)
3. [Setting up the software](#software)
4. [Add silent mode](#silent-mode)

# Materials <a name="materials"></a>
The following components have been used
- 1 NodeMCU v3 ESP8266 chip: https://aliexpress.com/item/32665100123.html
- 1 3v3 Relay: https://aliexpress.com/item/1005001567474787.html
- 230v AC to 5v DC converter: https://aliexpress.com/item/4000606845997.html
- Dupont/Jumper cables: https://aliexpress.com/item/4000203371860.html
- Existing doorbell 
- (Optional) 3D printed case: TBA

# Connecting the hardware <a name="hardware"></a>
1. Connect the wires as follows:
  - 5V AC/DC Converter >> Vin NodeMCU
  - Ground AC/DV Converter >> GND NodeMCU
  - Doorbell Wire 1 >> GND NodeMCU
  - Doorbell Wire 2 >> D5 NodeMCU
  - Relay In1 >> D1 NodeMCU
  - Relay Vcc >> 3V3 NodeMCU
  - Relay Gnd >> GND NodeMCU
2. Wire the doorbell with the relay, as follows:
```  
1. Empty
2. Power Cable from doorbell power supply
3. Power Cable to doorbell ringer
```
![alt text](https://i.imgur.com/IJ7CVIx.png "Wiring doorbell to relay")

3. Leave the ground cable of the doorbell directly connected with the doorbell rinnger

# Setting up the software <a name="software"></a>
1. Download Tasmotizer: https://github.com/tasmota/tasmotizer
2. Connect the NodeMCU via the USB port
3. In Tamotizer, select ```Release``` and click ```Tasmotize```
4. Look up an access point with the name ```tasmota_XXXXXX-####```, connect and follow the instructions
5. Go with the browser to the IP-adress of your NodeMCU
6. Click ```Configuration```
7. Click ```Configure Module```
8. Set ```Module Type``` to ```Generic```
9. Save and wait for the device to reboot
10. Repeat step 6 & 7
11. Configure the module as shown on the picture below

![alt text](https://i.imgur.com/WojE5Qz.png "Configuration Tasmota module")

12. Save and wait for the device to reboot

# Optional: add silent mode to doorbell <a name="silent-mode"></a>
Optionally, it is possible by adding a 'silent mode' to your doorbell. You do this by updating the configuration add temporarily remove the relay from the configuration.

## Home Assistant configuration (requires MQTT to be configured)
```
Switch:
- platform: mqtt
  name: "Deurbel stille modus"
  command_topic: "cmnd/doorbell/GPIO5" ## Set 'doorbell' to the topic name of your device
  state_topic: "stat/doorbell/RESULT" ## Set 'doorbell' to the topic name of your device 
  payload_on: "0"
  payload_off: "256"
  state_on: '{"GPIO0":{"0":"None"},"GPIO1":{"0":"None"},"GPIO2":{"0":"None"},"GPIO3":{"0":"None"},"GPIO4":{"0":"None"},"GPIO5":{"0":"None"},"GPIO9":{"0":"None"},"GPIO10":{"0":"None"},"GPIO12":{"0":"None"},"GPIO13":{"0":"None"},"GPIO14":{"160":"Switch1"},"GPIO15":{"0":"None"},"GPIO16":{"0":"None"},"GPIO17":{"0":"None"}}}'
  state_off: '{"GPIO0":{"0":"None"},"GPIO1":{"0":"None"},"GPIO2":{"0":"None"},"GPIO3":{"0":"None"},"GPIO4":{"0":"None"},"GPIO5":{"256":"Relay_i1"},"GPIO9":{"0":"None"},"GPIO10":{"0":"None"},"GPIO12":{"0":"None"},"GPIO13":{"0":"None"},"GPIO14":{"160":"Switch1"},"GPIO15":{"0":"None"},"GPIO16":{"0":"None"},"GPIO17":{"0":"None"}}}'
```
