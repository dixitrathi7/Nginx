# 📘 Understanding Nginx Configuration Files

### 📁 1. Nginx Configuration Directory: /etc/nginx/

All Nginx configuration files are stored in this directory 
```bash
/etc/nginx/
```

This directory typically contains:

- ```nginx.conf``` – the main configuration file

- ```sites-available/``` – store the configuration files for all the websites you want to host on your server.  (used in Debian/Ubuntu)

- ```sites-enabled/``` – This directory contains symbolic links to the configuration files located in the ```sites-available``` directory

- ```conf.d/``` – additional configuration files (used in most distros)

### ⚙️ 2. Main Configuration File: /etc/nginx/nginx.conf

This is the top-level context, outside of any blocks, where global settings for the entire NGINX server are defined. When Nginx starts, it reads this file first.

### 📂 3. sites-available/ and sites-enabled/ (Debian/Ubuntu)
These folders are used for organizing site configurations.

🔹 ```sites-available/```

Contains configuration files for all available websites

You can define multiple server blocks (virtual hosts) here

🔹 ```sites-enabled/```

Contains only the active sites

Usually, it contains symbolic links to files in sites-available/

This allows easy enable/disable of sites

 **📝 How to enable a site:**

```sudo ln -s /etc/nginx/sites-available/mysite /etc/nginx/sites-enabled/```

**🔁 How to disable a site:**

```sudo rm /etc/nginx/sites-enabled/mysite```


This approach is unique to Debian-based systems (Ubuntu). On other distros like CentOS, you typically use conf.d/ instead.

### 📁 4. conf.d/ Directory

This is another directory used to store additional config files, especially on RHEL/CentOS based systems.

All ```.conf``` files placed in ```/etc/nginx/conf.d/``` are automatically included by ```nginx.conf```.

It’s a good place to add custom global settings or separate site configurations.

## 🔁 5. What is a Reverse Proxy in Nginx?

A **reverse proxy** is a server that forwards client requests to another server, typically running on a different port or machine.

Nginx sits in **front of backend** services (like Node.js, Python, or Java apps) and handles incoming web requests. It can:

Forward requests to a local app e.g., ```localhost:3000```

Add **SSL support, caching, compression**, and more

Protect your app from direct public access

### 🧪 Configure Nginx as a Reverse Proxy to ```localhost:3000```

Let’s say you have a Node.js or Python app running at ```http://localhost:3000```. You want Nginx to serve it at ```http://mysite.com```.

Sample config (```/etc/nginx/sites-available/mysite.conf```):
```bash

server {
        listen 80;

        server_name mysite.com;

        location / {
                proxy_pass http://localhost:3000;

                # These headers are important for passing information to the backend application
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
        }

```
**Steps to enable:**

Save the file to ```/etc/nginx/sites-available/mysite```

Create a symlink in sites-enabled:

```bash
sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/
```

Test and reload Nginx:

```bash
sudo nginx -t
sudo systemctl reload nginx
```


Now, when someone visits ```http://mysite.com```, Nginx will forward the request to ```http://localhost:3000```.
