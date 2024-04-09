# Tutorial: Setting Up a Reverse Proxy with Nginx and Firewall with UFW on Arch Linux

This tutorial will guide you through setting up a reverse proxy using Nginx and configuring a firewall using UFW on an Arch Linux server. We will set up a backend service that runs on port 8080 and is accessible via the reverse proxy.

## Prerequisites

- An Arch Linux server running on DigitalOcean or any other cloud provider.
- Basic knowledge of Linux command line.
- SSH access to your server.

## Step 1: Install Necessary Software

We need to install Nginx and UFW. Nginx will act as our reverse proxy, and UFW will manage our firewall settings.
`sudo pacman -Syu nginx ufw`


## Step 2: Place the Backend Binary on Your System and move it to a suitable folder 

First, we need to transfer the downloaded backup file from your host machine to your linux server 

Connect to your droplet through sftp using `sftp -i ~/.ssh/do-key you_username@ip`

Use put with the path to the file to move the file to your droplet

For me it's `put C:\Users\HP\downloads\hello-server`

Now, SSH back into your droplet and move the file to this path using `sudo mv ~/hello-server /usr/local/bin/hello-server`


## Step 3: Create a New Service File for the Backend

Create a new systemd service file for your backend. This will allow it to run as a service.

Open a vim file in this location:
`sudo vim /etc/systemd/system/backend.service`

Add the following to the file:
```bash
[Unit]
Description= Backend Service
After=network.target

[Service]
ExecStart=/usr/local/bin/hello-server
Restart=always

[Install]
WantedBy=multi-user.target
```

Enable and start the service:

`sudo systemctl enable backend`
`sudo systemctl start backend`


## Step 4: Configure Nginx for Reverse Proxy

Create a new Nginx server block configuration file for your backend.

Open the vim file from part 1:
`sudo vim /etc/nginx/sites-available/nginx-2420`

Add the bottom two location routes to the file:

It should look like this now:
```bash
listen 80;
listen [::]:80;

server_name -;
root /web/html/nginx-2420;

location / {
    index  index.html index.htm;
}

location /hey {
    proxy_pass http://127.0.0.1:8080;
}

location /echo {
    proxy_pass http://127.0.0.1:8080;
}
```

Test the Nginx configuration and reload Nginx:
`sudo nginx -t`
 `systemctl reload nginx`


## Step 5: Configure UFW Firewall

Allow HTTP and HTTPS traffic through the firewall if you haven't previously:

`sudo ufw allow http`
`sudo ufw allow https`


Enable UFW using  `sudo ufw enable`

Press y and you should see this message "Firewall is active and enabled on system startup"


## Step 6: Test Your Backend

You can test your backend using postman 


## Step 7: Screenshots

Include screenshots demonstrating the successful connection to your backend. You can take screenshots of the terminal showing the successful `curl` commands or of the browser displaying the expected output from your backend routes.

## Conclusion

Congratulations! You have now set up a reverse proxy with Nginx and configured a firewall using UFW on your Arch Linux server. 
