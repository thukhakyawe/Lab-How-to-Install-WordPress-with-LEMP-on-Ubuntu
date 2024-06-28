# Lab-How-to-Install-WordPress-with-LEMP-on-Ubuntu

- How To Install Linux, Nginx, MySQL, PHP (LEMP stack) on Ubuntu

### Step 1 - Create Ubuntu EC2 ###

- I will not show this step. Please set up in AWS Account.
After EC2 creation finished like this, you can proceed the following steps.

![alt text](image.png)

### Step 2 - Install the Nginx Web Server

- Click `Connect` by selecting EC2 Server

![alt text](image-1.png)

- Click `Connect`

![alt text](image-2.png)

- Type ```sudo apt update``` in terminal and enter

![alt text](image-3.png)

![alt text](image-4.png)

- Type ```sudo apt install nginx``` in terminal and enter and Type ```y```

![alt text](image-5.png)

![alt text](image-6.png)

- Type ```sudo ufw app list``` in terminal and enter

![alt text](image-7.png)

- Type ```sudo ufw allow 'Nginx HTTP'``` in terminal and enter

![alt text](image-8.png)

- Type ```sudo ufw status``` in terminal and enter

![alt text](image-9.png)

- Type ```sudo ufw enable``` in terminal and enter and Type ```y```

![alt text](image-10.png)

- Type ```sudo ufw status``` in terminal and enter

![alt text](image-11.png)

- Type ```ip addr show``` in terminal and enter

![alt text](image-12.png)

- Type ```hostname -I``` in terminal and enter

![alt text](image-13.png)

- Write IP address of EC2 in web browser 

![alt text](image-14.png)

- Will see Nginx Page like this  

![alt text](image-15.png)

### Step 3 - Install MySQL

- Type ```sudo apt install mysql-server``` in terminal and enter
and type ```y```

![alt text](image-16.png)

![alt text](image-17.png)

- Type ```sudo mysql_secure_installation``` in terminal and enter and type ```y```

![alt text](image-18.png)

- Type `1` and enter

![alt text](image-19.png)

- Type `n` and enter

![alt text](image-20.png)

- Type `n` and enter

![alt text](image-21.png)

- Type `n` and enter

![alt text](image-22.png)

- Type `y` and enter

![alt text](image-23.png)

- Type ```sudo mysql``` in terminal and enter

![alt text](image-24.png)

- Type ```exit``` in terminal and enter

![alt text](image-25.png)

### Step 4 - Install PHP

-Type ```sudo apt install php-fpm php-mysql``` in terminal and enter and Type ```y``` and enter

![alt text](image-26.png)

![alt text](image-27.png)

### Step 5 - Configure Nginx to Use the PHP Processor

- Type ```sudo mkdir /var/www/wordpress_domain``` in terminal and enter

![alt text](image-28.png)

- Type ```sudo chown -R $USER:$USER /var/www/wordpress_domain``` in terminal and enter

![alt text](image-29.png)

- Type ```sudo nano /etc/nginx/sites-available/wordpress_domain``` in terminal and enter

- Fill the following information and save the file

```                                                        
server {
    listen 80;
    server_name 13.250.99.40;
    root /var/www/wordpress_domain;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}

```

![alt text](image-30.png)

- Type ```sudo ln -s /etc/nginx/sites-available/wordpress_domain /etc/nginx/sites-enabled/``` in terminal and enter

![alt text](image-31.png)

- Type ```sudo unlink /etc/nginx/sites-enabled/default``` in terminal and enter

![alt text](image-32.png)

- Note: If you ever need to restore the default configuration, you can do so by recreating the symbolic link, like the following:

```sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/```

- Type ```sudo nginx -t``` in terminal and enter

![alt text](image-33.png)

- Type ```sudo systemctl reload nginx``` in terminal and enter

![alt text](image-34.png)

- Type ```nano /var/www/wordpress_domain/index.html``` in terminal and enter

- Fill the following information and save the file

```
<html>
  <head>
    <title>Wordpress Website</title>
  </head>
  <body>
    <h1>Hello World!</h1>

    <p>This is the landing page of <strong>Wordpress Website</strong>.</p>
  </body>
</html>
```

![alt text](image-35.png)

- Write IP address of EC2 in web browser 

![alt text](image-36.png)

### Step 6 - Test PHP with Nginx

- Type ```nano /var/www/wordpress_domain/info.php``` and enter

![alt text](image-38.png)

- Fill the following information and save the file

```                                                       
<?php
phpinfo();
```

![alt text](image-37.png)

- Write IP address of EC2 in web browser and update link like this 

```http://13.250.99.40/info.php```

![alt text](image-39.png)

- Type ```sudo rm /var/www/wordpress_domain/info.php``` to remove

### Step 7 - Create a MySQL Database and User for WordPress

- Type ```sudo mysql``` in terminal and enter

![alt text](image-40.png)

- Type ```CREATE DATABASE example_database;``` in terminal and enter

![alt text](image-41.png)

- Type ```CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'P@ssword123';``` in terminal and enter

![alt text](image-42.png)

- Type ```GRANT ALL ON example_database.* TO 'example_user'@'%';``` in terminal and enter

![alt text](image-43.png)

- Type ```exit``` in terminal and enter

![alt text](image-44.png)

- Type ```mysql -u example_user -p``` in terminal and enter

![alt text](image-45.png)

- Type ```SHOW DATABASES;``` in terminal and enter

![alt text](image-46.png)

- Type ```exit``` in terminal and enter

![alt text](image-47.png)

### Step 8 - Install Additional PHP Extensions

- Type ```sudo apt update``` in terminal and enter

![alt text](image-48.png)

- Type ```sudo apt install php-curl php-gd php-intl php-mbstring php-soap php-xml php-xmlrpc php-zip``` in terminal and enter ```y``` and enter

![alt text](image-49.png)

![alt text](image-50.png)

- Type ```sudo systemctl restart php8.3-fpm``` in terminal and enter

![alt text](image-51.png)

- Type ```sudo nano /etc/nginx/sites-available/wordpress``` in terminal and enter

![alt text](image-53.png)

- Fill the following information and save the file


```
server {
    . . .

    location = /favicon.ico { log_not_found off; access_log off; }
    location = /robots.txt { log_not_found off; access_log off; allow all; }
    location ~* \.(css|gif|ico|jpeg|jpg|js|png)$ {
        expires max;
        log_not_found off;
    }
    . . .
}
```

![alt text](image-52.png)

- Type  ```sudo nginx -t``` in terminal and enter

![alt text](image-54.png)

- Type ```sudo systemctl reload nginx``` in terminal and enter

![alt text](image-55.png)

### Step 9 - Download WordPress

- Type ```cd /tmp``` in terminal and enter

![alt text](image-56.png)

- Type ```curl -LO https://wordpress.org/latest.tar.gz``` in terminal and enter

![alt text](image-57.png)

- Type ```tar xzvf latest.tar.gz``` in terminal and enter

![alt text](image-58.png)

- Type ```cp /tmp/wordpress/wp-config-sample.php /tmp/wordpress/wp-config.php``` in terminal and enter

![alt text](image-59.png)

- Type ```sudo cp -a /tmp/wordpress/. /var/www/wordpress_domain/wordpress``` in terminal and enter

![alt text](image-60.png)

- Type ```sudo chown -R www-data:www-data /var/www/wordpress_domain/wordpress```  in terminal and enter

![alt text](image-61.png)

### Step 10 - Set up the WordPress Configuration File

- Type ```curl -s https://api.wordpress.org/secret-key/1.1/salt/``` in terminal and enter

![alt text](image-62.png)

- Copy these information to notepad 

```
define('AUTH_KEY',         '+9J=c/h^w)q}Ef9IG40XD|gl6=P[Qej1MQd*|T}S+GUZ?(-DiQdd:kO5gk-3 |0G');
define('SECURE_AUTH_KEY',  'GD=yYxIfNs$h?O1;:VS}_|&jkG,_]bo#u]%2s}:vlVI.)%]P6!n<gnG*,_&*UGga');
define('LOGGED_IN_KEY',    'y3*Qk3+~6u{t|gVx>G*R->^>=Ad0ANuabGb0mqFtzS+abfuqJXP&j0x%]N<C?[]?');
define('NONCE_KEY',        '><7-<SrclrV8:vgJ(nwV0P34qs^]|2CGllbnh:v7&AK-(-ip+3rwP]p4|muB5Jd_');
define('AUTH_SALT',        'NOGza*IZb(-*sL?:j:y:VJx^FsmuF*jlQXci&.Rvp6m`73`m1p_^g[o(?|VY38)`');
define('SECURE_AUTH_SALT', '(PUl}fDkAZ)XT.+0?r,KZ:YGy#*2=J=9n!k:tsZzs;-wpC/<&!}|({isz;e%de3+');
define('LOGGED_IN_SALT',   'YqrY4v6q[3-PP5AX&#d&wG;X^^,jFX?[BRsxgV?eNe47yH3(S#-1cpsqZO>7-hh2');
define('NONCE_SALT',       '`Y!l-txV:0>-Xh#<|{n8!Zr*6>|{}r.-|pW#TPVR@mh8tA0!H>n&fF=0z~c!l|Yq');
```

- Type ```sudo nano /var/www/wordpress_domain/wordpress/wp-config.php``` in terminal and enter

![alt text](image-63.png)

- Update these information with the information in notepad

![alt text](image-64.png)

- Update these information with you created earlier and save file

![alt text](image-65.png)

### Step 11- Complete the Installation Through the Web Interface

- Type ```http://13.250.99.40/wordpress/wp-admin/install.php``` in terminal and enter and click `Continue`

![alt text](image-66.png)

- Fill the information and click `Install Wordpress`

![alt text](image-67.png)

- Click `Log In`

![alt text](image-68.png)

- Fill the information and Click `Log In`

![alt text](image-69.png)

-  You will see WordPress administration dashboard

![alt text](image-70.png)


---

***Congratulations, you have completed Lab - How To Install Wordpress with LEMP on Ubuntu***

---


