Setup Pi Zero
=============

Ansible scripts to setup a Raspberry Pi.

Requirements
------------

Needs a Raspbian SD card mounted at /Volumes/boot and a working ansible
installation.

### Write SD card image on Mac

Download Raspbian image from 'https://www.raspberrypi.org/downloads/raspbian/'
and use the following commands to write it to the card. This is a dangerous
operation and make sure that your SD card is on `/dev/disk2`.

```
$ diskutil list # Note which disk to write to
$ sudo dd if=files/2017-04-10-raspbian-jessie-lite.img of=/dev/disk2 bs=32m
```

Create WiFi config file
-----------------------

Copy the file `files/wpa_supplicant.conf.sample` to
`files/wpa_supplicant.conf` and edit it with your WiFi details.

Prepare SD card
---------------

Run this playbook while the SD card is still mounted at `/Volumes/boot`.

The playbook will setup SSH, WiFi, Ethernet over USB and disable bluetooth.

```
$ ansible-playbook setup_sdcard.yml -l localhost
```

Setup Raspberry Pi
------------------

Unmount the SD card and insert it to the Raspberry Pi and boot it up. Then run
the playbook `setup_pizero.yml` to update the Raspbian system and install some
default packages.

It will ask for a hostname to set on the Raspberry Pi, just press enter to
keep it as the default `raspberrypi.local`.

Make sure you have an SSH key in `~/.ssh/id_rsa` as the playbook will setup
it as an authorized key and disable password login via SSH.

```
$ ansible-playbook setup_pizero.yml -l raspberrypi.local -i inventory
```

