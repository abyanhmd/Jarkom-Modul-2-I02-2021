# Jarkom-Modul-2-I02-2021

Made by:

Abyan Ahmad (05111942000013)

Gede Yoga Arisudana (05111942000009)

Zulfiqar Rahman Aji (05111942000019)

The Prefix IP of our group **192.209**.

## Problem 10
This problem asks us to make another subdomain named **super.franky.i02.com** with similar config like previous problem. The difference is only like this

```
ServerName super.franky.i02.com
ServerAlias www.super.franky.i02.com
ServerAdmin webmaster@localhost
DocumentRoot /var/www/super.franky.i02.com/
```

![10](Screenshot/10to13.png)

We also need to make a new folder in _var/www/_ named **super.franky.i02.com**. And we need to run these commands

```
a2ensite super.franky.i02.com
service apache2 restart
```

We can check it using one of the client with `lynx super.franky.i02.com`.

![10 lynx](Screenshot/10_lynx.png)

<br>

## Problem 11
Directory listing can be solved using these commands.

```
<Directory /var/www/super.franky.i02.com/public>
        Options +Indexes
</Directory>
```

We can check it using one of the client with `lynx super.franky.i02.com`.

![11 lynx](Screenshot/11_lynx.png)

<br>

## Problem 12
Changing error page can be done using this command.

```
ErrorDocument 404 /error/404.html
```

We can check it using one of the client with `lynx super.franky.i02.com/[randomstring]`.

![12 lynx](Screenshot/12_lynx.png)

<br>

## Problem 13
This problem can be solved using these commands.

```
<Directory /var/www/super.franky.i02.com/public/js>
        Options +Indexes
</Directory>

Alias "/js" "/var/www/super.franky.i02.com/public/js"
```

We can check it using one of the client with `lynx super.franky.i02.com/js`.

![13 lynx](Screenshot/13_lynx.png)

<br>

## Problem 14
This problem asks us to make another subdomain named **general.mecha.franky.i02.com** with similar config like previous problem but can only be accessed with 15000 port and 15500 port. The config will be like this

```
<VirtualHost *:15000 *:15500>
    ServerName general.mecha.franky.i02.com
    ServerAlias www.general.mecha.franky.i02.com
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/general.mecha.franky.i02.com/
</VirtualHost>
```
We also need to change _/etc/apache2/ports.conf_ and add these to it.

```
Listen 15000
Listen 15500
```
After that, we need to run these commands in terminal.

```
a2ensite general.mecha.franky.i02.com
service apache2 restart
```
We can check it using one of the client with `lynx www.general.mecha.franky.i02.com:15000`.

![14 lynx](Screenshot/14_lynx.png)

<br>

## Problem 15
In order to make authentication, we can add these commands in **general.mecha.franky.i02.com.conf**.

```
<Directory /var/www/general.mecha.franky.i02.com>
        AuthType Basic
        AuthName "Private"
        AuthBasicProvider file
        AuthUserFile "/etc/apache2/.htpasswd"
        Require valid-user
        Options +Indexes
        AllowOverride All
</Directory>
```
Username and password can be added using this command in terminal.

![15 UserPass](Screenshot/15_userpass.png)

After using this command `lynx www.general.mecha.franky.i02.com:15000`, it will ask for username and password.

![15 user](Screenshot/15_user.png)

![15_pass](Screenshot/15_pass.png)

After input password, it will show the page like the previous problem.

<br>

## Problem 16
If we input IP of Skypie, it will direct the page to apache index.html page. It implies that it uses _000-default.conf_ to open up the page. So, we can change the DocumentRoot in _000-default.conf_ to **franky.i02.com**.

```
ServerAdmin webmaster@localhost
DocumentRoot /var/www/franky.i02.com
```

<br>

## Problem 17
In order to solve this problem, we need to add some lines of code and .htaccess in the DocumentRoot. The lines of code is added in _/etc/apache2/sites-available/super.franky.i02.com.conf_.

```
<Directory /var/www/super.franky.i02.com>
        Options +Indexes +FollowSymLinks -MultiViews
        AllowOverride All
</Directory>
```

For the .htaccess, we can make regular expression to find the substring of _franky_. The regular expression in the .htaccess can be like this.

```
RewriteEngine On
RewriteRule /public/images/(.*)franky(.*)\.(jpg|png)$ http://super.franky.i02.com/public/images/franky.png [L]
```
The first parameter of RewriteRule is the regular expression which the extension of the file is specified as jpg or png. The second parameter is the location of franky.png in the webpage. The flag L indicates that franky.png is the last destination. After writing this .htaccess, we need to run these commands.

```
a2enmod rewrite
service apache2 restart
```

We can check it using one of the client with `lynx super.franky.i02.com/public/images`.

![17 lynx](Screenshot/17_lynx.png)
