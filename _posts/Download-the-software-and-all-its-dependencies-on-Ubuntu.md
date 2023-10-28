---
title: Download the software and all its dependencies on Ubuntu
date: 2023-04-03 19:06:58
tags:
declare: true
---

##### 1. Install apt-rdepends first
`sudo apt install apt-rdepends`

##### 2. Download all dependencies
`apt download $(apt-rdepends openssh-server | grep -v "^ ")`

##### 3. If an error `E: Can't select candidate version from package debconf-2.0 as it has no candidate` is reported
`apt download $(apt-rdepends openssh-server | grep -v "^ " | sed 's/debconf-2.0/debconf/g')`
This command will successfully download **openssh-server** and all required packages and save them in **/var/cache/apt/archives** directory.

##### 4. Installtion
`sudo dpkg -i openssh-server*.deb`
even `sudo dpkg -i *`

