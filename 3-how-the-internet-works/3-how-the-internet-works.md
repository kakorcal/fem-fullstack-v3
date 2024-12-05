# How the internet works

(Cloudflare tutorials)[https://www.cloudflare.com/learning/]

# Request flow

Computer
  -> Network card
    -> Router
    -> Modem (maybe)
    -> ISP
    -> Tier 1 ISP 
    -> Tier 1 ISP again
    -> Data center
    -> Server cluster
    -> Load balancer
    -> Server
    -> Return response

# Request flow DNS

Computer
  -> Browser
    -> Find closest nameserver
      -> Return IP address
    -> Request flow

# Terms

* Internet - network of networks
* Intranet - private network
* LAN - local area network
* WAN - Wide area network
* IP - internet protocol
* IP address - numeric label assigned to an internet connected device
  * IPv4 - 8.8.8.8
  * IPv6 - 2001:4860:4860:8888
* TCP - transmission control protocol (most reliable - establishes handshake)
* UDP - user datagram protocol (one way data sending - faster, less bandwidth, good for streaming)
* ICMP - Internet control message protocol
* Packet - unit of data transmitted over a network
* DNS - Domain name system
* Nameserver - hold DNS records to translate domain names into IP addresses
* Domain name registrar - company that allows you to register a domain name
* A record - maps domain name to IP address (typically sites will have more than one IP address to balance the load)
* CNAME - maps domain name to another name (so you don't need multiple IP addresses)
* URL - uniform resource locator (https://frontendmasters.com/courses/fullstack-v3/dns-urls/)
* TLD - top level domain
* Cybersquatting - the illegal act of registering or using a domain name that is similar or identical to a trademark, company name, or personal name to profit from it
* ICANN - The Internet Corporation for Assigned Names and Numbers (ICANN) is a non-profit organization that manages the internet's domain name system (DNS) and IP addresses
* root - highest permission level. Allows unrestricted access to the OS
* sudo - super user do. Allows you to run commands and programs as root

# Network tools

* Check status of a network request (is the host up or down?)
  * `ping google.com`
* Follow the path of your request (prints request flow)
  * `traceroute google.com`
* Show network status (all network stats from your computer)
  * `netstat -lt | less`
* Lookup the nameservers for a domain
  * `nslookup frontendmasters.com`
* Lookup the DNS records for a domain
  * `dig frontendmasters.com`
* View browser DNS cache
  * chrome://net-internals/#dns
  * about:networking#dns

# Anatomy of a URL

blog.yourdomain.com/en/fullstack?test=true

* Subdomain - blog.yourdomain.com (fyi www. is a considered a subdomain)
* Domain - yourdomain.com
* TLD - com
* Path - /en/fullstack
* Query parameter - ?test=true

# Brand new server setup

1. Update software
  * ssh into your server
  * For Ubuntu, `apt` is used for package management - `apt update && apt upgrade`
    * `update` is sortof like updating package.json
    * `upgrade` is sortof like updating node_modules
2. Restart server
  * `shutdown now -r`
3. Create a new user
  * `root` and `#` indicate you are logged in as `root` ex. `root@ubuntu-11111:~#`
  * We don't want to operate as a root user because none of the commands are double-checked
  * Create new user for Ubuntu - `adduser <your_username>`
4. Make that user a superuser
  * Add user to sudo group - `usermod -aG sudo <your_username>`
  * Switch user - `su <your_username>`
  * Check sudo access of that new user - `sudo cat /var/log/auth.log`
  * Note that you can only see the auth.log file as a root user
5. Enable login for new user - we need create an authorized_keys file which the new user will use to login
  * Create authorized_keys file - `~/.ssh/authorized_keys`
  * Paste public key into authorized_keys
  * Exit server
  * Login with new user - `ssh <your_name>@<your_ip>`
6. Disable root login - so that you can never ssh into it as root
  * Change file permission (only the new user can write to it) - `chmod 644 ~/.ssh/authorized_keys`
  * Disable root login by updating ssh daemon config file - `sudo vi /etc/ssh/sshd_config`
    * `etc` folder will include configs
    * Update `PermitRootLogin` to be `no`
  * Daemon configs are only read once at runtime so we need to restart ssh daemon - `sudo service ssh restart` or `sudo systemctl restart ssh`
  * Might need to restart the server at this point
7. Switch back to root if needed
  * `sudo -i`
