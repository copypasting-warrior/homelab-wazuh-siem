# homelab-wazuh-siem

## Overview

An old thinkpad x230 which has been repurposed to host a Wazuh SIEM 
server on Arch in order to provide secure monitoring of devices on
home network.

## Why this project?

I wanted to monitor devices on my home network in order to prevent 
any intrusions.There also happened to be an old Thinkpad x230 which 
was not being used since 2020 .So to knock out two birds with one stone
i repurposed the Thinkpad x230 to function as a headless Wazuh Server
that can monitor local devices.

## Hurdles:

1.Due to the Thinkpad x230 being relatively lower in terms of processing power
,I had to pay attention to resources used.
2.Wazuh does not natively support Arch due to a lack of apt or yum causing the error:

>ERROR: Couldn't find YUM or APT package manager. Try installing the one 
>corresponding to your operating system.


3.Had to ensure the server was not accessible from outside the network as
it reduces the chance of malicious people gaining access to it

## My solutions:

1.To reduce load i decided to use Arch as it is one of the lighter and more
bare bones Linux distros that would not consume as many resources.

2.Due to us choosing Arch which uses pacman instead of APT or YUM we had to choose between:
A)Find an AUR
B)Use the Wazuh Docker version

I decided to go with B as its more well documented and reduces  friction.

3.To ensure the server was not accessible from outside the local network we switch of upnp 
and virtual server settings from our router panel and also establish a uncomplicated firewall
(ufw) on our server.

<img width="567" height="254" alt="Screenshot 2026-08-31 175312" src="https://github.com/user-attachments/assets/be926960-c562-4117-ad08-99057fc250db" />

UPNP disabling

<img width="560" height="194" alt="Screenshot 2026-08-31 175319" src="https://github.com/user-attachments/assets/2c8a6c79-de75-4140-b080-ca496e746f31" />

No virtual server

<img width="611" height="257" alt="screenshot2" src="https://github.com/user-attachments/assets/bb0b6323-5a24-49cf-97b5-38ad0ee458be" />

The ufw rules
##Steps:
this is a brief step by step explanation of what i did:
1)Refurbished and fixed the old Thinkpad x230 by replacing its drained CMOS and adding a battery

2)Backed up any important data and installed Arch Linux on the system using a pen drive and archinstall script

3)Logged into router panel and adjusted so upnp and virtual servers werent enabled as well as assigned a static address
for our server so agents can send data to it.

<img width="540" height="154" alt="Screenshot 2026-08-31 180023" src="https://github.com/user-attachments/assets/dd1131e1-766a-48f4-b27b-a266329be324" />

4.Checked available resources on server using :
\'''
free -h
\'''

<img width="427" height="117" alt="screenshot" src="https://github.com/user-attachments/assets/a3b3c012-a017-48fa-bf76-8c14be042a92" />

and then checked local subnet using
\'''
ip addr Show
\'''

5.Then I installed and Setup ufw and set the rules as:
>Default deny incoming
>
>Default allow outgoing
>
>Allow from 192.168.1.0/24 to 443,1514,1515 and 5500

6.Install and configure fail2ban to protect server from brute force attacks(redundant).

7.Install Docker and wazuh for docker to host the manager,indexer and dashboard and then generate certificates and
set the docker instance up.

<img width="1316" height="325" alt="screenshot3_1" src="https://github.com/user-attachments/assets/18f425a1-1b8b-4da0-bc1b-a7145e04ad9d" />

8.Change default credentials by hashing new password using tools in the wazuh indexer container
 and replacing default password in the internal_users.yml file.
 
9.Open Wazuh dashboard by accessing server ip address on host computer and adding agents to monitor devices.

<img width="1242" height="680" alt="Screenshot 2026-09-02 230028" src="https://github.com/user-attachments/assets/6904eb8f-3ec4-455d-ac45-c3be7190f056" />

##Future considerations:
1.Switching setup to Debian to host Wazuh completely on the machine.

2.Add users and groups with different privileges to ssh more securely.

3.Add Suricata to monitor the network as well.

4.Simulate intrusions to check effectiveness.

  
