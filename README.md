# Web Server Deployment Using Nginx
## Overview

This project provides a step-by-step guide to deploying a web server using Nginx. The deployment includes setting up Nginx as a reverse proxy, configuring server blocks, and ensuring secure access using SSL/TLS.

## Prerequisites

Before proceeding with the deployment, ensure you have the following:

* A Linux-based server (Ubuntu, CentOS, Debian, etc.)

* Root or sudo access

* A registered domain name (optional but recommended)

* An application or website to serve

## Installation Steps

### <p align ="">Step 1: Update System Packages</p>

```bash
sudo apt update && sudo apt upgrade -y   # For Ubuntu/Debian
sudo yum update -y                       # For CentOS/RHEL
```

### <p align ="">Step 2: Install Nginx</p>

```bash
sudo apt install nginx -y    # For Ubuntu/Debian
sudo yum install nginx -y    # For CentOS/RHEL
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
sudo nano /etc/nginx/sites-available/example.com
```

Add the following configuration:

```bash
server {
    listen 80;
    server_name example.com www.example.com;

    root /var/www/example.com;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Enable the site and restart Nginx:

```bash
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

## Testing Deployment

1. Open a web browser and visit http://example.com

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

- Ensure the domain DNS is correctly configured

## Conclusion

By following this guide, you have successfully deployed a web server using Nginx. You can further optimize the setup by adding caching, load balancing, or integrating it with Docker and CI/CD pipelines.
