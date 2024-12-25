# Security

## Terminology

* Port - communication endpoint that maps to specific process or network service. Ports allow a machine to have multiple services on one IP address
* Open port - a network port that's configured to accept packets from a TCP or UDP connection, allowing communication with a server
* Firewall - Monitors incoming network traffic and determines whether to allow or block
* ufw - uncomplicated firewall (software to help configure)

## Commands

* See well known ports - `less /etc/services`
* Read auth log - `sudo cat /var/log/auth.log`

## nmap

* Install nmap - `sudo apt install nmap`
* nmap - `nmap <your_server_ip>`
* Extra service/version information - `nmap -sV <your_server_ip>`

## ufw

firewall manager. Abstracts out ip config table 

* Silently reject a port (preferred) - `ufw deny <port>`
* Explicitly reject a port (not preferred since you need to send error packets. When you send packets you are susceptible to DDoS attacks) - `ufw reject <port>`
* Allow a port - `ufw allow <port>`

### ufw workflow

1. Check firewall status - `sudo ufw status`
2. Allow ssh - `sudo ufw allow ssh`
3. Allow http - `sudo ufw allow http`
4. Enable firewall - `sudo ufw enable`

## Permissions

Chmod uses octal numbers

Read -> 4
Write -> 2
Execute -> 1

Each rwx max is 7 per group. 

| Owner | Group | Everyone else
| ----- | ----- | -------------
|  rwx  |  rwx  |     rwx

So if you do `chmod 600 <file_name>`, the permissions will be:
rw- --- --- (only the owner can read and write. no one can execute)

[chmod cheatsheet](https://quickref.me/chmod)

## Application updates

It is important to keep up with software updates (LTS versions only) to prevent vulnerabilities.

### unattended-upgrades

1. Install - `sudo apt install unattended-upgrades`
2. Enable upgrades - `sudo dpkg-reconfigure --priority=low unattended-upgrades`

## Checklist

* SSH key
* Firewalls
* Updates
* Two factor authentication
* VPN

