# HLS Devices - Thermocouple12
Setup instructions for the HSL Thermocouple12 Board.

## Setup

1. Connect via UART with username `pi` and password `raspberry`.
2. Change default password from `raspberry` to whatever you want. This can be done running `passwd` or using `raspi-config`.
3. Change host name from `hls-device-thermocouple-12-unknown` to whatever you want. This can be done by running `sudo su && echo "hls-device-thermocouple-12-a" > /etc/hostname` echoing your desired hostname.
4. (optional) Enable SSH either through `raspi-config` or running `systemctl enable ssh.socket`.
5. Reboot to see changes. (`sudo reboot`)
6. Get latest setup script `cd /home/pi/hls-devices-thermocouple-12/ && git pull`.
7. Run setup script `cd /home/pi/hls-devices-thermocouple-12/ && bash setup.sh` and answer prompts.
8. Reboot to see changes. (`sudo reboot`)

Model: Thermocouple12
Rev: 2.0
Unit: ______________ 
Date: ______________ 