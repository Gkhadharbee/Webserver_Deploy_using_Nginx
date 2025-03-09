# Web Server Deployment Using Nginx
## Overview

This project provides a step-by-step guide to deploying a web server using Nginx. The deployment includes setting up Nginx as a reverse proxy, configuring server blocks, and ensuring secure access using SSL/TLS.

## Prerequisites

Before proceeding with the deployment, ensure you have the following:

* A Linux-based server (Ubuntu, CentOS, Debian, etc.)

* Root or sudo access

* An application or website to serve

## Installation Steps

### <p align ="">Step 1: Update System Packages</p>

```bash
sudo apt update && sudo apt upgrade -y   # For Ubuntu/Debian
sudo yum update -y                       # For CentOS/RHEL
```

### <p align ="">Step 2: Install Nginx</p>

```bash
sudo apt install nginx -y                 # For Ubuntu/Debian
sudo amazon-linux-extras enable nginx1    # For CentOS/RHEL
sudo yum install -y nginx

```

### <p align ="">Step 3: Start and Enable Nginx</p>

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

### <p align ="">Step 4: Configure Firewall</p>

```bash
sudo ufw allow 'Nginx Full'  # For Ubuntu/Debian
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload   # For CentOS/RHEL
```

### <p align ="">Step 5: Configure Nginx Server Blocks</p>

Create a new configuration file:

```bash
sudo vim /etc/nginx/conf.d/default.conf
```

Add the following configuration:

```bash
server {
    listen 80;
    server_name 54.225.58.185;  # your IP-address

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

### <p align ="">Step 6: Secure Nginx with SSL</p>
To enable HTTPS, install and configure Let's Encrypt SSL: 

```bash
sudo apt install certbot python3-certbot-nginx -y            # If you have a domain
sudo certbot --nginx -d example.com -d www.example.com
```

- Renew SSL automatically:

```bash
sudo certbot renew --dry-run
```

If you don't have a domain, you can generate a self-signed SSL certificate instead:

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt
```

- Then, configure Nginx to use this certificate:

```bash
server {
    listen 443 ssl;
    server_name 54.225.58.185;     # your IP address
    
    ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
    ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;

    root /var/www/html;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

- Restart Nginx:

```bash
sudo systemctl restart nginx
```

### <p align="">Step 7: Deploy Your Website</p>
Remove the default Amazon Linux page:

```bash
sudo rm -f /usr/share/nginx/html/index.html
```

Create a new index page:

```bash
echo "<h1>Welcome to My Nginx Web Server</h1><p>This is simple project about the Web Server Deployment using Nginx"</p> | sudo tee /usr/share/nginx/html/index.html
```

## Testing Deployment

1. Open http://your-ec2-public-ip in your browser

2. Verify Nginx is serving the expected content

3. Check Nginx status using:

```bash
sudo systemctl status nginx
```

## Troubleshooting

- Check Nginx logs for errors:

```bash
sudo tail -f /var/log/nginx/error.log
```

- Verify firewall settings

## Conclusion

Finally, you have successfully deployed a web server using Nginx. 
