
# WSL2 Setup for Local Web Development

This tutorial is a short and straight-forward guide on installing WSL2 and other software that can works with web development. The purpose is honestly as a note to myself but feel free to use this if you happen to develop a web on your local machine.

I will teach you how to install the following:

	WLS2 + Ubuntu
	Ubuntu
	PHP
	Nginx
	MySQL
	NodeJS + NPM

## 1. Preparations

To get started you need to make sure that you already enabled and your machine meet the requirements below:

1. You must be running Windows 10 version 2004 and higher (Build 19041 and higher) or Windows 11 -- 	*[According to the official pre-requisites](https://learn.microsoft.com/en-us/windows/wsl/install#prerequisites)*

2. WSL2 requires virtualization to be enabled on your PC. To check you can run powershell and run `systeminfo.exe` and find for **Virtualization Enabled In Firmware: X**. If you happen to see **No**, You can enable it by going to BIOS. Please refer to your motherboard model manual.

3. Recommended specification:

		Memory: 8GB
		Storage: 256GB

4. You need to enable 2 features before get started to install WSL2. Run **Win + R** and type in `optionalfeatures`. Now refer to the image below to enable 2 features which is:
		Virtual Machine Platform
		Windows Subsystem for Linux

	![Optional Features](https://i.imgur.com/Jp4C4pc.png)

## 2. Lets Get Started - WSL2 Installation

You can now install everything you need to run WSL with a single command.

1. Open powershell **(Run as Administrator)** and type the command `wsl --install`
2. By default, the installed Linux distribution will be Ubuntu.
3. Please wait for a while until it prompt you to input your UNIX username and password.
	-- [Best practices for setting up a WSL development environment](https://learn.microsoft.com/en-us/windows/wsl/setup/environment#set-up-your-linux-username-and-password)

## 3. Making Sure Everything Is Installed

Now we installed WSL2 + Ubuntu. WSL can be access with powershell each time you restart by just typing `wsl`.

Before installing new software, it's always a good idea to update the package lists and upgrade the installed packages to the latest versions.

1. `sudo apt update`
2. `sudo apt upgrade -y `
	`-y` will answer every prompt with **Yes**.

## 4. Installing PHP + Other Dependencies

Now we need to install the most famous server-side scripting language for web development **PHP**. We also need to install other packages that depends on PHP:

	php
	php-cli
	php-fpm
	php-mysql

1. Run the command `sudo apt install -y php php-cli php-fpm php-mysql`
2. Wait for a while and after everything is completed you can run `php -v`

## 5. Installing NodeJS + NPM

Now lets install **NodeJS** and its package manager which is **Node Package Manager**.

1. Run the command `curl -sL https://deb.nodesource.com/setup_20.x | sudo -E bash -`
2. Now we got the package, run `sudo apt install -y nodejs` to install nodejs.

## 6. Installing MySQL

Now lets install our one and only database management system **MySQL**. 

1. Run the command `sudo apt install -y mysql-server`
2. Complete the setup wizard where it will ask you to enter the root password.
3. Now we got MySQL install, let start the service with `sudo service start mysql`
4. It is recommended to run `sudo mysql_secure_installation` to optimize your mysql security.

## 7. Installing Nginx

Now lets install the most important service to make our web works, **Nginx**.

1. Run the command `sudo apt install -y nginx`
2. Now after done we can start the nginx service using `sudo service nginx start`

Now lets configure our web server to works with PHP. By default nginx should works with HTML.

1. Navigate to default configuration in nginx folder using `sudo nano /etc/nginx/sites-available/default`
2. Get rid of everything and paste this configuration snippets:
		server {
			listen 80 default_server;
        	listen [::]:80 default_server;
    
        	root /var/www/html;
        	index index.php index.html index.htm index.nginx-debian.html;
    
        	server_name _;
    
        	location / {
            	try_files $uri $uri/ =404;
        	}
    
        	location ~ \.php$ {
            	include snippets/fastcgi-php.conf;
            	fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
        	}
    
        	location ~ /\.ht {
            	deny all;
        	}
		}

3. Now lets restart nginx using `sudo service nginx restart`

## 8. Done, Lets Code

You are done now. You are now able to use directory `/var/www/` to develop your website. Open your favourite IDE and connect to remote directory and start coding.

## 9. Useful Reference

1. [Microsoft WSL2 Setup Guide](https://learn.microsoft.com/en-us/windows/wsl/setup/environment#set-up-your-linux-username-and-password)
2. [Microsoft WSL2 Pre-Requisites](https://learn.microsoft.com/en-us/windows/wsl/install#prerequisites)
