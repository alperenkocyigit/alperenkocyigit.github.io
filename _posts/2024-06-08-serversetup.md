---
title: How To Setup New Server With Nginx (Angular-Ruby and R-Python)
date: 2024-06-07 12:00:00 -500
categories: [Tutorials]
tags: [alperenkocyigit,howto]
---

# Common Instalations

## System Update
```bash
    sudo apt update
```

```bash
    apt install mc
```

## Install Nginx
```bash
    sudo apt install nginx
```

* Check nginx status
```bash
    systemctl status nginx
```

```bash
    sudo nano /etc/nginx/sites-available/'your_domain'
```

* /etc/nginx/sites-available/your_domain

```
    server {
            listen 80;
            listen [::]:80;

            root /var/www/your_domain;
            index index.html index.htm index.nginx-debian.html;

            server_name your_domain www.your_domain;

            location / {
                    try_files $uri $uri/ =404;
            }
    }
```

```bash
    sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
```

# Angular-Ruby Server Side

## Angular

* Move your angular project inside to `/var/www/your_domain` path

## Ruby

TODO

# R-Python Server Side

TODO