---
title: wsl-guide
date: 2022-03-18 19:40:10
tags:
- ubuntu
- wsl
- docker
categories:
- DevOps
---

## Install WSL
TODO
![test](wsl-guide/test.jpg)
## Network
### Connect Cisco Anyconnect VPN
1. Connect Cisco Anyconnect VPN, then open up powershell as Administrator and get DNS by following commands:
```powershelll
Get-DnsClientServerAddress -AddressFamily IPv4 | Select-Object -ExpandProperty ServerAddresses
```
2. On the same powershell running the following to get the search domain:
```powershell
Get-DnsClientGlobalSetting | Select-Object -ExpandProperty SuffixSearchList
```
3. Open WSL, running the below commands:
```shell
sudo unlink /etc/resolv.conf # this will unlink the default wsl2 resolv.conf
# This config will prevent wsl2 from overwritting the resolve.conf file everytime
# you start wsl2
cat <<EOF | sudo tee -a /etc/wsl.conf
[network]
generateResolvConf = false
EOF

cat <<EOF | sudo tee -a /etc/resolv.conf
nameserver 10.50.1.2 # The company DNS/nameserver from the command in step 1
nameserver 10.50.3.4 # The company DNS/nameserver from the command in step 1
nameserver 8.8.8.8
nameserver 8.8.4.4
search searchdomain.com # The search domain that we got from step 2
EOF
```
4. Change Cisco Anyconnect metric from default 1 to 6000 in powershell
```powershell
Get-NetAdapter | Where-Object {$_.InterfaceDescription -Match "Cisco AnyConnect"} | Set-NetIPInterface -InterfaceMetric 6000
```
5. Restart WSL in same powershell
```powershell
Restart-Service LxssManager
```