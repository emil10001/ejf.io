+++
date = "2016-03-07T08:37:43-08:00"
title = "Let's Encrypt - setting up automated certificate renewals"

+++

<img alt="let's encrypt" src="https://storage.googleapis.com/ejf-io/letsencrypt-logo-horizontal.svg">

Not too long ago, Let's Encrypt launched a free CA to enable website owners to easily generate their own signed certificates from the command line on their server. It integrates with popular web servers like Apache and NGINX to make the validation step easier. However, the certificates are only valid for 3 months, as opposed to the 1 year that is more typical. They'll send an email when your certificate is close to expiring, but that's not always idea. The good news is that since this is a command line tool, it can be easily written into a cron job to run periodically.

The current release, 0.4.2, seems to work reasonably well for scripted renewals. There are a couple notes for issues that I ran into, at least with this version. First, here's the general command that I use for renewals:

    ./letsencrypt/letsencrypt-auto certonly --no-self-upgrade --apache --force-renew --renew-by-default --agree-tos -nvv --email admin@example.com -d example.com,www.example.com >> /var/log/letsencrypt/renew.log

That's great when you have a simple site, with sub-domains that all point to one set of content, and one vhost entry (per port). If you have a couple of different subdomains that relate to different sets of content, something like this:

    ./letsencrypt/letsencrypt-auto certonly --no-self-upgrade --apache --force-renew --renew-by-default --agree-tos -nvv --email admin@example.com -d example.com,www.example.com,app.example.com >> /var/log/letsencrypt/renew.log

Where `app.example.com` in the above example relates to a different vhost. In this case, you'll need to break the vhosts into separate files. See the below examples.

Vhost for `example.com` and `www.example.com`:

    <VirtualHost *:80>
        ServerAdmin admin@example.com
        ServerName example.com
        ServerAlias www.example.com
        DocumentRoot "/var/www/example/"
        <Directory "/var/www/example/">
            DirectoryIndex index.html
            RewriteEngine On
            RewriteOptions Inherit
            AllowOverride All
            Order Deny,Allow
            Allow from all
            Require all granted
        </Directory>
    </VirtualHost>

    <VirtualHost *:443>
        ServerAdmin admin@example.com   
        ServerName example.com
        ServerAlias www.example.com  
        DocumentRoot "/var/www/example/"
        <Directory "/var/www/example/">
            AllowOverride all
            Options -MultiViews +Indexes +FollowSymLinks
            DirectoryIndex index.html index.htm
            Order Deny,Allow
            Allow from all
            AllowOverride All
            Require all granted
            DirectoryIndex index.html index.php index.phtml index.htm
        </Directory>

        ErrorLog "/var/log/httpd/example.com-error_log"
        CustomLog "/var/log/httpd/example.com-access_log" common

        SSLEngine on

        SSLCertificateFile /etc/letsencrypt/live/example.com/fullchain.pem
        SSLCertificateKeyFile /etc/letsencrypt/live/example.com/privkey.pem
    </VirtualHost>

Vhost for `app.example.com`:

    <VirtualHost *:80>
        ServerAdmin admin@example.com
        ServerName app.example.com
        DocumentRoot "/var/www/example-app/"
        <Directory "/var/www/example-app/">
            DirectoryIndex index.html
            RewriteEngine On
            RewriteOptions Inherit
            AllowOverride All
            Order Deny,Allow
            Allow from all
            Require all granted
        </Directory>
    </VirtualHost>

    <VirtualHost *:443>
        ServerAdmin admin@example.com   
        ServerName app.example.com
        DocumentRoot "/var/www/example-app/"
        <Directory "/var/www/example-app/">
            AllowOverride all
            Options -MultiViews +Indexes +FollowSymLinks
            DirectoryIndex index.html index.htm
            Order Deny,Allow
            Allow from all
            AllowOverride All
            Require all granted
            DirectoryIndex index.html index.php index.phtml index.htm
        </Directory>

        ErrorLog "/var/log/httpd/example-app.com-error_log"
        CustomLog "/var/log/httpd/example-app.com-access_log" common

        SSLEngine on

        SSLCertificateFile /etc/letsencrypt/live/example.com/fullchain.pem
        SSLCertificateKeyFile /etc/letsencrypt/live/example.com/privkey.pem
    </VirtualHost>

Then, I created a script that handles a couple of different renewals, and at the end, a command to reload apache configs, called `updateCerts.sh`:

    #!/bin/bash

    ./letsencrypt/letsencrypt-auto certonly --no-self-upgrade --apache --force-renew --renew-by-default --agree-tos -nvv --email admin@example.com -d example.com,www.example.com,app.example.com >> /var/log/letsencrypt/renew.log
    ./letsencrypt/letsencrypt-auto certonly --no-self-upgrade --apache --force-renew --renew-by-default --agree-tos -nvv --email admin@thing.com -d thing.com,www.thing.com,files.thing.com >> /var/log/letsencrypt/renew.log

    apachectl graceful

Then, I added this to the root user's crontab:

    # m h  dom mon dow   command
    0 4  *   *   1     /home/user/updateCerts.sh

Cron is supposed to run this script once a week, and we'll have logs at `/var/log/letsencrypt/renew.log`.
