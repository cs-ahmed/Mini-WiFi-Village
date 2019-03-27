# Raspberry Pi (RPi) Setup for WiFi Village

This document allows you to setup one RPi to connect to the 5GHz `LONDON` network and keep generating traffic on the network. Before you proceed, make sure you labeled this RPi as `LONDON` to avoid future confusions! Once Kali is up and running, perform the following actions in the same order.

### Connect to the target WiFi network

Connect to the WiFi network that the students should attack

Enter the correct password

### Find out the Gateway IP address

Open a terminal and run `route -n`

Note the IP address for the Gateway.

The Gateway IP address will be used in the next section as the IP dst in the `send` command.

> In the next section I will assume that the Gateway IP address is `192.168.0.1`

### Create a py file to generate traffic on the target network

Step 1:

Open a terminal and go to ```/root/```

Step 2:

Create a python file ```LONDON.py```

Step 3:

Run:
```chmod +x LONDON.py```


Open the python file and type:

```#! /usr/bin/env python
from scapy.all import send, IP, ICMP
import time
while True:
	send(IP(dst="192.168.0.1")/ICMP()/"testICMPpacket", count=100000)
	time.sleep(30)
```

> Make sure to substitute the `192.168.0.1` with the Gateway IP address you noted earlier in the *Find out the Gateway IP address* section directions.

### Make the script run the python program

Go to ```/root/```

Create a ```script.sh```

run:
```chmod +x script.sh```

Open script.sh and type:

```#!/bin/sh
sleep 80
sudo python start.py
```


### Set Kali to run the script you just made on boot

In the terminal, go to ```/etc/```

Run ```gedit crontab```

At the bottom of the file type:
```@reboot /root/script.sh &```

Save and exit

Run ```sudo crontab -e```

At the bottom of the file type:
```@reboot /root/script.sh &```

### Restart and you are good to go!

You are now set. All you need is to restart your RPi. About 2 minutes after you restart the `LONDON` RPi, you should see traffic on the `LONDON` network. To verify that, use a Kali Vm on any other machine with a wireless card connected to it to run the following command:

`airodump-ng -c 149 --essid LONDON wlan0mon`

You should see something like this:

![LONDON-traffic](./images/LONDON-traffic.png)

### Furthermore

From now on, you can keep the `LONDON` network broadcasting and just turn your `LONDON` RPi on to generate traffic or off to stop the traffic.

---

Acknowledgment: [Michael Benos](mtb9ps@virginia.edu) came up with and documented most of the directions in this document.

&copy; Ahmed Ibrahim, 2019

This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC BY-NC-SA 4.0). To view a copy of this license, visit https://creativecommons.org/licenses/by-nc-sa/4.0/.

The information contained herein are provided on an "AS IS" basis and THE CONTRIBUTOR, THE ORGANIZATION HE/SHE REPRESENTS OR IS SPONSORED BY (IF ANY) DISCLAIM ALL WARRANTIES, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO ANY WARRANTY THAT THE USE OF THE INFORMATION HEREIN WILL NOT INFRINGE ANY RIGHTS OR ANY IMPLIED WARRANTIES OF MERCHANTABILITY OR FITNESS FOR A PARTICULAR PURPOSE.
