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

### Create Deploy User

```bash
    adduser deploy
```
```bash
    adduser deploy sudo
```

### Install PostgreSQL

```bash
    sudo apt-get update
```

```bash
    sudo apt-get install postgresql postgresql-contrib libpq-dev
```

# R-Python Server Side

TODO