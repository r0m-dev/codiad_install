# Codiad quick install in Debian 10

Install prerequisite

```bash
apt-get update && apt-get install git -yy
nano /etc/apache2/sites-available/codiad.lan.local.conf
```

Create vhost for apache and in this configuration, i allow only my public IP address.

```bash
<VirtualHost *:80>
        ServerName codiad.lan.local
        DocumentRoot "/usr/share/Codiad"
        ErrorLog "/var/log/apache2/codiad.lan.local-errors.log"
        CustomLog /var/log/apache2/codiad.lan.local-access.log combined

         <Directory /usr/share/Codiad>
                Options Indexes FollowSymLinks MultiViews
                AllowOverride all
                Order deny,allow
                allow from xx.xx.xx.xx
                allow from 127.0.0.1
                allow from yy.yy.yy.yy
        </Directory>
</VirtualHost>
```

Download and install Codiad
```bash
cd /usr/share
git clone https://github.com/Codiad/Codiad.git
cd Codiad
cp config.example.php config.php
Dans le fichier config à modifier :
define("WHITEPATHS", BASE_PATH . ",/home,/var/www/,/usr/share/");
```
Rights modification

```bash
chown -R www-data:www-data config.php workspace/ plugins/ themes/ data/
```

Add vhost and reload apache conf
```bash
a2ensite codiad.lan.local
service apache2 restart
```

Bug correction with recent php version

```bash
sed -i 's!\(mb_ord\)!codiad_\1!g;s!\(mb_chr\)!codiad_\1!g' /usr/share/Codiad/lib/diff_match_patch.php
```
