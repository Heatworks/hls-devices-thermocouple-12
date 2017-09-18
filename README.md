# HLS Devices - Thermocouple12
Code and setup instructions for the HSL Thermocouple12 Board.

## Setup

1. Connect via UART with username `pi` and password `raspberry`.
2. Change default password from `raspberry` to whatever you want. This can be done using `sudo raspi-config` and pressing enter or running `passwd`.
3. Change host name from `hls-device-thermocouple-12-unknown` to whatever you want. This can be done by running `sudo su && echo "hls-device-thermocouple-12-a" > /etc/hostname` echoing your desired hostname.
4. (optional) Enable SSH either through `raspi-config` or running `systemctl enable ssh.socket`.
5. Reboot to see changes. (`sudo reboot`)
6. Get latest setup script `cd /home/pi/hls-devices-thermocouple-12/ && git pull`.
7. Run setup script `cd /home/pi/hls-devices-thermocouple-12/ && bash setup.sh` and answer prompts.
8. Reboot to see changes. (`sudo reboot`)

## Base Image
The following are the instructions for creating the base image for the Thermocouple12 board, using a copy of this image will contain all the following edits. Begin with the default raspbian lite image.

- Ethernet: Add `dtoverlay=enc28j60,int_pin=25,speed=12000000` to `/boot/config.txt` as per [this](http://raspi.tv/2015/ethernet-on-pi-zero-how-to-put-an-ethernet-port-on-your-pi) tutorial. 
- Set Hostname to `hls-device-thermocouple-12-unknown`.
- Enable `serial` (USART) hardware.

General updates, node, and npm.

```
sudo apt-get update
sudo apt-get install node npm git

sudo npm cache clean -f
sudo npm install -g n
sudo n 6.* 
sudo npm install -g node-red
```

### Node-Red

Begin by running node red, this generates the flow and settings files.

Edit `~/.node-red/settings.js` and change the `flowFile` option to `flows_default.json`.

### Enable SPI

Enable SPI using `sudo raspi-config`.

### Add Code for MAX31855

```bash
cd ~/
sudo apt-get install python-dev
git clone https://github.com/adafruit/Adafruit_Python_GPIO.git
cd Adafruit_Python_GPIO
sudo python setup.py install

cd ~/
sudo apt-get install build-essential python-dev python-smbus
git clone https://github.com/adafruit/Adafruit_Python_MAX31855.git
cd Adafruit_Python_MAX31855
sudo python setup.py install

cd ~/.node-red
sudo npm install @heatworks/node-red-contrib-adafruit-max31855

```

### Add this Repo

Clone this repo and setup environment variables in `/home/pi/.profile`.

```
export NODE_RED_CRED_FILE=/home/pi/.node-red/flows_default_cred.json
export NODE_RED_FLOW_FILE=/home/pi/.node-red/flows_default.json
export NODE_RED_SETTINGS_FILE=/home/pi/.node-red/settings.js
```

Run through setup once with default values. May need to remove `~/.node-red/.config.json` file as it may have automatically generated credentials.

### CronTab

Open crontab `crontab -e` and add this line `@reboot bash /home/pi/hls-devices-thermocouple-12/startup.sh > /home/pi/startup.log 2>&1`.

### Create Log File

```
sudo touch /var/log/node-red.log
sudo chown pi:pi /var/log/node-red.log 
```

## Future

- Include password and host name changes in the prompt setup script.
- Run setup through UART.