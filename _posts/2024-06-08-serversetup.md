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

```bash
    sudo systemctl restart nginx
```

* Done!!

## Ruby

### Create Deploy User First

```bash
    adduser deploy
```
```bash
    adduser deploy sudo
```
* Change to deploy user before installing ruby!!

### Install Ruby

```bash
    sudo apt install git curl autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev
```

```bash
    curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-installer | bash
```

* You need to add $HOME/.rbenv/bin to your PATH environment variable to start using Rbenv.

    - If you are using the Bash shell, use this command:
        ```
        echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
        echo 'eval "$(rbenv init -)"' >> ~/.bashrc
        source ~/.bashrc
        ```
    - If you are using the Zsh shell, use this command:
        ```
        echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc
        echo 'eval "$(rbenv init -)"' >> ~/.zshrc
        source ~/.zshrc
        ```
* Verify the installation by checking the version of Rbenv:

    ```
    rbenv -v
    ```

    You can check the outputs of `rbenv -v` , `ruby -v` , `rbenv local` , `rbenv global`

```bash
    rbenv install --list-all
```

```bash
    rbenv install 3.2.2
```

```bash
    rbenv global 3.2.2
```

```bash
    > ruby --version
    ruby 3.2.2 (2023-03-30 revision e51014f9c0) [x86_64-linux]
```

### Install Rails

```bash
    gem search '^rails$' --all
```

```bash
    gem install rails -v 7.0.8
```

```bash
    rbenv rehash
```

```bash
    rails -v
```

### Install PostgreSQL

```bash
    sudo apt-get update
```
```bash
    sudo apt-get install postgresql postgresql-contrib libpq-dev
```
* Create Database
```bash
    sudo apt-get install postgresql postgresql-contrib libpq-dev
    sudo su - postgres
    createuser --pwprompt deploy
    createdb -O deploy myapp
    exit
```

### Configure Nginx Config File

```etc/nginx/sites-available/appname
upstream puma {
    server unix:/var/www/your-path/shared/sockets/puma.sock fail_timeout=0;
}

server {
    listen 80;
    server_name your-path;

    root /var/www/your-path/public;

    access_log /var/www/your-path/log/nginx.access.log;
    error_log /var/www/your-path/log/nginx.error.log info;

    location ^~ /assets/ {
        gzip_static on;
        expires max;
        add_header Cache-Control public;
    }

    try_files $uri/index.html $uri @puma;
    location @puma {
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_redirect off;
        proxy_pass http://puma;
    }

    error_page 500 502 503 504 /500.html;
    client_max_body_size 4G;
    keepalive_timeout 10;
}
```

```bash
    sudo ln -s /etc/nginx/sites-available/appname /etc/nginx/sites-enabled/
```

### Add Systemctl puma.service File
* sudo nano /etc/systemd/system/puma.service
```shell
    [Unit]
    Description=Rails Puma Server
    After=network.target

    [Service]
    User=deploy

    WorkingDirectory=/var/www/your_app_path

    ExecStart=/home/deploy/.rbenv/shims/bundle exec puma -C /var/www/your_app_path/config/puma.rb

    PIDFile=/var/www/your_app_path/shared/pids/puma.pid
    Restart=always
    [Install]
    WantedBy=multi-user.target
```

## Install Ruby Packages
Install essential packages
* Pandoc
```shell
    sudo apt-get install pandoc
```
* Pdf-Latex
```shell
    sudo apt-get install texlive-latex-base texlive-fonts-recommended texlive-fonts-extra texlive-latex-extra
```
* Texlive-Xetex For `pdf_engine: :xelatex`
```shell
    sudo apt-get install texlive-xetex
```

### Last Steps
* Change to root path of app
```bash
    bundle install
```

* Add config/credentials/production.key
* Check credentials
```bash
    EDITOR='nano' rails credentials:edit --environment production
```
* Set env to production
```bash
    export RAILS_ENV=production
```
* Create Puma socket file structre
```bash
    mkdir /var/www/app-path/shared
    mkdir /var/www/app-path/shared/sockets
    mkdir /var/www/app-path/shared/pids
    mkdir /var/www/app-path/shared/log
```

* Run Puma Server With Configuration
```bash
    systemctl daemon-reload
    systemctl start puma.service
    systemctl status puma.service
```

# R-Python Server Side

TODO