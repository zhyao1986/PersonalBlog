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
### Reference
https://cloud-atlas.readthedocs.io/zh_CN/latest/linux/ubuntu_linux/network/wpa_supplicant.html