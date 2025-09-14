# 📘 Section 2: How to Use Nginx to Expose Your Website on the Internet

### ✅ Step 1: Check and Start the Nginx Service
Make sure Nginx is installed and running.

```bash
sudo systemctl status nginx
sudo systemctl enable nginx
```
### 📁 Step 2: Copy Your Website Content

Copy your website's main HTML file into Nginx’s default web directory:

```bash
sudo cp index.html /var/www/html/index.html

```
### 📁 Default Web Root in Linux

| Directory             | Purpose                          |
|-----------------------|----------------------------------|
| `/var/www/html`       | Default directory for static files |
| `/etc/nginx/sites-available/default` | Default config file pointing to the web root |


### 🔄 Step 3: Reload Nginx

After updating the website files, reload Nginx to apply changes:
```bash
sudo systemctl reload nginx
```

### 🌐 Access Your Website

Now your website is live! Open a browser and visit:
```bash
http://<your-server-ip>
```
