
Webserver setup on **ec2-18-210-18-191.compute-1.amazonaws.com**
```
server {
    listen 80;
    server_name ec2-18-210-18-191.compute-1.amazonaws.com;
    root /var/www/ec2-18-210-18-191.compute-1.amazonaws.com;
    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ /index.php$is_args$args;
    }

    location = /favicon.ico { log_not_found off; access_log off; }
    location = /robots.txt { log_not_found off; access_log off; allow all; }
    location ~* \.(css|gif|ico|jpeg|jpg|js|png)$ {
        expires max;
        log_not_found off;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
```




LetEncrpyt certificate (**ACME server refuses to issue a certificate for this domain name, because it is forbidden by policy**:
```
An unexpected error occurred:
The server will not issue certificates for the identifier :: Error creating new order :: 
Cannot issue for "ec2-18-210-18-191.compute-1.amazonaws.com": 
The ACME server refuses to issue a certificate for this domain name, because it is forbidden by policy
Ask for help or search for solutions at https://community.letsencrypt.org. See the logfile /var/log/letsencrypt/letsencrypt.log or re-run Certbot with -v for more details.

```

Database setup on **ip-172-31-53-115.ec2.internal**

Wordpress database setup:
```
mysql> CREATE DATABASE wordpress_db;
Query OK, 1 row affected (0.00 sec)

mysql> CREATE USER 'wp_user'@'ip-172-31-62-101.ec2.internal' IDENTIFIED BY 'password';
Query OK, 0 rows affected (0.02 sec)

mysql> GRANT ALL ON wordpress_db.* TO 'wp_user'@'ip-172-31-62-101.ec2.internal'
    -> ;
Query OK, 0 rows affected (0.00 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)

mysql> exit
Bye
```


