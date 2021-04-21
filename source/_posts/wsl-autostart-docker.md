---
title: Enable docker autostart in WSL
date: 2021-04-21 22:35:15
tags:
- ubuntu
- wsl
- docker
categories:
- DevOps
---
## Enable docker autostart in WSL
### WSL
* Create shell script "/etc/init-docker.sh" as below
```shell
#start docker service
sudo service docker start
```
* Set execute permission to script
`sudo chmod +x /etc/init-docker.sh`
### Windows
* Go to "startup" folder with execute "shell:startup" in "Run"/"Win + R"
* Create file "start.vbs" as below
```vb
Set ws = WScript.CreateObject("WScript.Shell")
ws.run "wsl -d ubuntu -u root /etc/init-docker.sh", vbhide
```
