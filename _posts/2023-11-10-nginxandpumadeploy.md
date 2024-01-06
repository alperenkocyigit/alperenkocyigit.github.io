---
title: How To Deploy Rails API Only App With Nginx and Puma
date: 2023-11-10 12:00:00 -500
categories: [Tutorials,Ruby]
tags: [alperenkocyigit,ruby,rails,howto]
---

# How To Deploy Rails API Only App With Nginx and Puma
In this tutorial you can deploy api only rails app using nginx and puma.


## Creating a Deploy user

While logged in as root on the server, we can run the following commands to create the deploy user and add them to the sudo group.

```bash
    adduser deploy
    adduser deploy sudo
    exit
```
## Configure Database Connection
Ensure that you are in your application’s root directory (cd ~/appname).

Open your application’s database configuration file in your favorite text editor.

```bash
    nano config/database.yml
```

Update the production section so it looks something like this:

```yml
    production:
        <<: *default
        host: localhost
        adapter: postgresql
        encoding: utf8
        database: appname_production
        pool: 5
        username: <%= ENV['APPNAME_DATABASE_USER'] %>
        password: <%= ENV['APPNAME_DATABASE_PASSWORD'] %>
```
Save and exit.

## Install rbenv-vars Plugin

To install the rbenv-vars plugin, simply change to the .rbenv/plugins directory and clone it from GitHub. For example, if rbenv is installed in your home directory, run these commands:

```bash
    cd ~/.rbenv/plugins
    git clone https://github.com/sstephenson/rbenv-vars.git
```
### Set Environment Variables
* Create Secret First
```bash
    cd ~/appname
    rake secret
```
> Note That secret
{: .prompt-warning }

### 

## Creating a PostgreSQL Database

```bash
    sudo apt-get install postgresql postgresql-contrib libpq-dev
    sudo su - postgres
    createuser --pwprompt deploy
    createdb -O deploy myapp
    exit
```
You can manually connect to your database anytime by running psql -U deploy -W -h 127.0.0.1 -d myapp. Make sure to use 127.0.0.1 when connecting to the database instead of localhost.

## Working Puma Config File
../config/puma.rb
```ruby
threads_count = ENV.fetch("RAILS_MAX_THREADS") { 5 }
threads threads_count, threads_count

environment ENV.fetch("RAILS_ENV") { "production" }

app_dir = File.expand_path("../..", __FILE__)
shared_dir = "#{app_dir}/shared"

# Soket ile Puma'yı başlatmak için
bind "unix://#{shared_dir}/sockets/puma.sock"

# Logging
stdout_redirect "#{shared_dir}/log/puma.stdout.log", "#{shared_dir}/log/puma.stderr.log", true

# PID ve State dosyaları
pidfile "#{shared_dir}/pids/puma.pid"
state_path "#{shared_dir}/pids/puma.state"

activate_control_app
```
Alternative: 

```ruby
  if ENV.fetch('RAILS_ENV') == 'production'
    threads_count = ENV.fetch("RAILS_MAX_THREADS") { 5 }
    threads threads_count, threads_count
    environment ENV.fetch("RAILS_ENV") { "production" }
    app_dir = File.expand_path("../..", __FILE__)
    shared_dir = "#{app_dir}/shared"
    # Soket ile Puma'yı başlatmak için
    bind "unix://#{shared_dir}/sockets/puma.sock"
    # Logging
    stdout_redirect "#{shared_dir}/log/puma.stdout.log", "#{shared_dir}/log/puma.stderr.log", true
    # PID ve State dosyaları
    pidfile "#{shared_dir}/pids/puma.pid"
    state_path "#{shared_dir}/pids/puma.state"
    activate_control_app
  else
    max_threads_count = ENV.fetch('RAILS_MAX_THREADS') { 5 }
    min_threads_count = ENV.fetch('RAILS_MIN_THREADS') { max_threads_count }
    threads min_threads_count, max_threads_count
    worker_timeout 3600 if ENV.fetch('RAILS_ENV', 'development') == 'development'
    port ENV.fetch('PORT') { 3000 }
    environment ENV.fetch('RAILS_ENV') { 'development' }
    pidfile ENV.fetch('PIDFILE') { 'tmp/pids/server.pid' }
    # workers ENV.fetch("WEB_CONCURRENCY") { 2 }
    # preload_app!
    plugin :tmp_restart
  end
```

## Working Nginx Config File
../etc/nginx/sites-available/appname.conf
```ruby
upstream puma {
    server unix:/var/www/appname/shared/sockets/puma.sock fail_timeout=0;
}

server {
    listen 80;
    server_name appname;

    root /var/www/appname/public;

    access_log /var/www/appname/log/nginx.access.log;
    error_log /var/www/appname/log/nginx.error.log info;

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
* Reload nginx after configurations.
```bash
    systemctl reload nginx.service
```

## Install Packages
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

## Ban From IP
```shell
    iptables -A INPUT -s 192.168.1.1 -j DROP
```

## How to Run Puma Server With Comfiguration
```bash
    bundle exec puma -C ./config/puma.rb
```

Helper links: 
* https://www.digitalocean.com/community/tutorials/how-to-deploy-a-rails-app-with-puma-and-nginx-on-ubuntu-14-04#install-puma
* https://gorails.com/deploy/ubuntu/22.04#ruby

Now you successfully deploy api only rails project using puma and nginx.
```ruby
puts "Well Done!!"
```