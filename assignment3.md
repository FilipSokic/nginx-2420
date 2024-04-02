# Setting Up Nginx on Arch Linux with a Separate Server Block

This tutorial will guide you through setting up Nginx on a fresh Arch Linux server running on DigitalOcean, configuring it to serve a simple HTML document, and managing the Nginx service using systemd.

## Prerequisites

- A fresh Arch Linux server running on DigitalOcean.
- Basic knowledge of Linux command line.

## Step 1: Update Your System

First, ensure your system is up to date:

`sudo pacman -Syu`


## Step 2: Install Necessary Software

Install Nginx and Vim:

`sudo pacman -S nginx vim`


## Step 3: Start and Enable Nginx

Start the Nginx service 

`sudo systemctl start nginx`

Enable it to start on boot:

`sudo systemctl enable nginx`


## Step 4: Create Project Directory

Create a new directory for your project:

 `sudo mkdir -p /web/html/nginx-2420`


## Step 5: Configure Nginx

### Create a Separate Server Block

Create a new directory for the config file

`sudo mkdir -p /etc/nginx/sites-available`


Create a new server block configuration file:

`sudo vim /etc/nginx/sites-available/nginx-2420.conf`

Write the follwing in the file:

```bash
server {
    listen 80;
    server_name your_DigitalOcean_droplet_ip;

    location / {
        root /web/html/nginx-2420;
        index index.html;
    }
}
```
### Enable the Server Block

Create a symbolic link to enable the server block:

 sudo ln -s /etc/nginx/sites-available/nginx-2420.conf /etc/nginx/sites-enabled/


