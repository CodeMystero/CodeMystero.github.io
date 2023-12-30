---
layout: single

title: "[Linux] Samba installation"
excerpt: "creating share folder on window dir"

categories:
  - Linux server

tag: [C/C++, linux, kernel] 

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
---

![header](https://capsule-render.vercel.app/api?type=rect&color=20:660099,100:E2231A)


## Install samba

```bash
root@raspberrypi: # apt-get install samba samba-common-bin
root@raspberrypi: # samba -V
Version 4.17.12-Debian
```

## Set sambe password

```bash
root@raspberrypi:/project/objview # smbpasswd -a pi
```
## Open samba.conf

```bash
root@raspberrypi:/project/objview # cd /etc/
root@raspberrypi:/etc # cd samba/
root@raspberrypi:/etc/samba # ls
gdbcommands  smb.conf  tls
root@raspberrypi:/etc/samba # vi smb.conf
```

## Adding share dir details on samba.conf

find out the profile directory in samba.conf, then add below dir details

```bash
[raspu] #folder title
    comment = superuser
    path = /  #share from root file 
    valid users = pi #user name
    browseable = yes
    create mask = 0777 #authorization rwe
    directory mask = 0777
    writable = yes
```

## Restart smb

```bash
root@raspberrypi:/etc/samba # service smbd restart
```

"Now, if you enter the Raspberry Pi's IP address in the Windows Run dialog and log in, you will find the directory you configured. You can freely access the file system."
