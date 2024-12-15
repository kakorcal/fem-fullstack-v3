# App setup

## Add web server on top of your server

It's best practice to have a reverse proxy in front your application server. The web proxy will route traffic to the correct place. Nginx or Apache are the main choices.

* Request -> Nginx or Apache -> Go to Application or Database or another Server

## Nginx

* Installing Nginx - `sudo apt install nginx`
* Starting Nginx - `sudo service nginx start`
* View default config - `less /etc/nginx/sites-available/default`

### Nginx config
* `root /var/www/html;` - Base directory for requests
* `location / {...}` - Routes paths
* Directives - operations / commands within a location block. (ex. `try_files` tries to return some file back)

## NodeJS / Git

* Link to newest LTS node.js source - `curl https://deb.nodesource.com/setup_20.x | sudo -E bash -`
* Install node.js - `sudo apt-get install nodejs`
* Install git - `sudo apt install git` 

## Establish application project

* Change ownership of /www - `sudo chown -R $USER:$USER /var/www`
* Make an application directory - `mkdir /var/www/app`
* Initialize empty git repo in /app - `git init`
* Create app file - `touch app.js`
* Initialize node project - `npm init`

## Proxy pass

* Create new virtual nginx server to proxy requests - `sudo vi /etc/nginx/sites-enabled/my-domain-name`

### Creating a virtual nginx server

```
server {
  listen 80 default_server;
  listen [::]:80 default_server;

  root /var/www/html;
  index index.html;

  server_name my_server_name.com;

  location / {
    proxy_pass http://127.0.0.1:3000;
  }
}
```

* Validate your nginx config - `sudo nginx -t`
* Point nginx to new server - `sudo vi /etc/nginx/nginx.conf`

```
##
# Virtual Host Configs
##

include /etc/nginx/conf.d/*.conf;
include /etc/nginx/sites-enabled/my-domain-name;
```
* Restart nginx - `sudo service nginx restart`
* Start node server - `node app.js`

### Installing PM2

Process manager (PM) will keep the node.js server running (even if your laptop is closed)

1. Install PM2 - `sudo npm i -g pm2`
2. Start PM2 - `pm2 start app.js --watch`
3. Setup auto restart - `pm2 save && pm2 startup`
4. View processes - `pm2 list` 

## Git

1. Ensure git uses your new ssh key - `vi ~/.ssh/config`
2. Change permission of config to 600 - `chmod 600 ~/.ssh/config`
3. Change permission of github key to 600 - `chmod 600 ~/.ssh/<github_key>`

## Useful debugging commands

* Stop a running process - `pkill <process>`
* Test your ssh connection - `ssh -vT <ip_address>`
* Save a readonly file in vim - `:w !sudo tee %`
* View permissions as numbers - `stat -c %a <file_name>` 