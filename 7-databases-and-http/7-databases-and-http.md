# Databases

## Sqlite

Most common db that can be used in any platform: `npm install sqlite3`

### Prepared statement

The query and the data are sent to the database server separately to prevent [sql injection attacks](https://stackoverflow.com/questions/8263371/how-can-prepared-statements-protect-from-sql-injection-attacks).

### Clean up 

When server is finished we should always shutdown the db as well

```
process.on('SIGINT', () => {
  server.close(() => {
    db.close();
  });
});
```

## HTTP

Hypertext transport protocol - request and response

### Common request headers

* User-agent - the request device type
* Accept - what the device will handle
* Accept-language - Browser languages
* Content-type - the type of media
* Set-cookie - Sets stateful information
* X- - Custom headers

### Common response status codes

* 200 - ok
* 201 - successful post request (created successfully)
* 301 - Moved permanently
* 302 - Found (temporary redirect)
* 401 - Not authorized
* 500 - Internal server error

## HTTPS

Encrypts network packets during request and response cycle. You can't use https with just an IP address and a domain name is needed.

### Implement HTTPS with certbot

1. Install and use (certbot)[https://certbot.eff.org/instructions]

Install cert and create a [logical link](https://www.freecodecamp.org/news/linux-ln-how-to-create-a-symbolic-link-in-linux-example-bash-command/)

Certbot will update your `/etc/nginx/sites-enabled/<your-domain>` virtual host config file with cert settings

```
listen [::]:443 ssl ipv6only=on; # managed by Certbot
...
ssl_certificate /etc/letsencrypt/live/<your-domain>/fullchain.pem
ssl_certificate_key /etc/letsencrypt/live/<your-domain>/privkey.pem
```

* (What is SSL?)[https://www.cloudflare.com/learning/ssl/what-is-ssl/] - A website that implements SSL/TLS has "HTTPS" in its URL instead of "HTTP." SSL initiates an authentication process called a handshake between two communicating devices to ensure that both devices are really who they claim to be. An SSL certificate is like an ID card or a badge that proves someone is who they say they are. SSL certificates are stored and displayed on the Web by a website's or application's server.
* (What is PEM?)[https://medium.com/@yashschandra/anatomy-of-a-pem-file-727f1690df18] - file format for storing and sending cryptographic keys, certificates, and other data, based on a set of 1993 IETF standards defining “privacy-enhanced mail. It's similar usage to SSH public and private keys.

2. Open firewall - `sudo ufw allow https`

## HTTP versions

### HTTP/1.1

Every file request / response is a separate TCP connection

### HTTP/2

Multiplexing - multiple file requests on single TCP connection. Could be faster but requires more CPU usage

1. Update nginx server - `sudo vi /etc/nginx/sites-enabled/<your-domain>`

```
listen 443 http2 ssl;
```