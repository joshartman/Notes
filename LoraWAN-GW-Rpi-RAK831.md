# LoRa Basics™ Station Installation steps

These notes are for install the Semtech LoRa Basics™ Station on a Raspberry Pi Zero-W with a RAK831 LoRa board.

## Hardware

- Raspberry Pi Zero-W
  - A 32GB SD Card
- RAK831 loRa Gateway Module
- Raspberry PI Zero RAK831 Lora Gateway Shield [https://github.com/hallard/RAK831-Zero]


## Steps

1. Download the Firmware from the Semtech website.
   - [https://downloads.rakwireless.com/LoRa/RAK831-LoRa-Gateway/]
   - This eventually points to the RAK2247 which is the newer model.
2. Extract the .img from the ZIP file and write it to the SD card
   - I used `Raspberry Pi Imager` on a Mac to write to the SD card.
3. Once the write is finished, remove and re-attach the SD card.
   - Create an empty file called `ssh` to enable `ssh` login via the network
   - Create a file called `wpa_supplicant.conf` to enable wireless with the following content:
     ```
     country=CH
     ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
     update_config=1
     network={
       ssid="YOURSSID"
       scan_ssid=1
       psk="YOURPASSWORD"
       key_mgmt=WPA-PSK
     }
     ```
4. Unmount the SD card and boot the Raspberry Pi with it.
   - It might take a while to boot and connect to the network.
5. It will create an error when starting up (view the daemon.log file). This can be suppressed by editing `/usr/local/rak/ap/bin/create_ap` and add a line to `exit 0` immediately.
   ```
   #!/bin/bash
   
   exit 0
   
   # general dependencies:
   ...
   ```
7. Create a Gateway in the TTN console and download the `global_conf.json` file.
8. Copy the `global_conf.json` file to the RPi into the directory: `/opt/ttn-gateway/packet_forwarder/lora_pkt_fwd/global_conf.json`
9. If the board from Hallard is used, the reset GPIO is different from what the fireware expects.
   - Edit the script: `/opt/ttn-gateway/packet_forwarder/lora_pkt_fwd/start.sh` and change the `SX1301_RESET_BCM_PIN` to 25
   ```
   # Reset iC880a PIN
   SX1301_RESET_BCM_PIN=25  # 17
   ```
10. Reboot the RPi and it should connect to the TTN Node.
