---
title: Auto WIFI connect in Ubuntu
date: 2021-04-20 00:25:51
tags:
- ubuntu
categories:
- STARS
---

## Auto WIFI connect in Ubuntu
### Prepare
Use `iwconfig` to get name of wireless network interface adapter
`iwconfig`
* If the status of wireless network interface adapter is down, use `ifconfig` to enable
`sudo ifconfig wlan0 up`
* then scan network
`sudo iwlist wlan0 scan | grep ESSID`
### Connect WIFI
* Install wpa_supplicant
`sudo apt install -y wpasupplicant`
* Creat config file `wpa_supplicant.conf` in /etc/wpasupplicant/, content as below
```
ctrl_interface=/var/run/wpa_supplicant
ap_scan=1
network={
    ssid="401-ZG"
    psk="19860527"
    priority=1
}
network={
    ssid="Yao"
    psk="qwer1234"
    priority=2
}
```
* Connect WIFI
`sudo wpa_supplicant -B -c /etc/wpasupplicant/wpa_supplicant.conf -i wlan0`
* Get ip addresss
`sudo dhclient wlan0`
### Create auto-start service
* Create service file named `wpa_supplicant.service` in `/etc/systemd/system/`, content as below
```
[Unit]
Description=WPA supplicant
Before=network.target
After=dbus.service
Wants=network.target
IgnoreOnIsolate=true

[Service]
Type=dbus
BusName=fi.w1.wpa_supplicant1
ExecStart=/sbin/wpa_supplicant -u -s -c /etc/wpasupplicant/wpa_supplicant.conf -i wlan0

[Install]
WantedBy=multi-user.target
Alias=dbus-fi.w1.wpa_supplicant1.service
```
* Enable the service
`sudo systemctl enable wpa_supplicant.service`
### Set static IP
* Get config file
Using command `ls /etc/netplan`, in our ubuntu, the file name is "50-cloud-init.yaml"
* Edit file as below
```
# This file is generated from information provided by the datasource.  Changes
# to it will not persist across an instance reboot.  To disable cloud-init's
# network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
    ethernets:
        eth0:
            dhcp4: true
            optional: true
        wlan0:
            dhcp4: no
            addresses: [192.168.0.10/24]
            optional: true
            gateway4: 192.168.0.1
            nameservers:
                    addresses: [8.8.8.8,8.8.4.4,192.168.0.1]
    version: 2
```
* Apply config with `sudo netplan apply`
* Check result with `ip a`
### Reference
https://pimylifeup.com/ubuntu-20-04-static-ip-address/
https://cloud-atlas.readthedocs.io/zh_CN/latest/linux/ubuntu_linux/network/wpa_supplicant.html