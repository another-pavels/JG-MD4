
## 1. Gatava darbam infrastruktūra.
![Infrastucture component](https://github.com/pavljiks/JG-MD4/blob/main/report_pictures/Selection_386.png?raw=true)


## 2. Bilde ar Wordpress lapu un serveru sarakstu Jūsu repozitorija.
![Main page](https://github.com/pavljiks/JG-MD4/blob/main/report_pictures/Selection_385.png?raw=true)


## 3. Apraksts MD formātā Jūsu repozitorija.

Šis pats fails: https://github.com/pavljiks/JG-MD4/blob/main/report.md

## 4. Jūsu arhitektūras risinājums no diagrams.net jābūt Jūsu repozitorija.
![Main page](https://github.com/pavljiks/JG-MD4/blob/main/report_pictures/aws-schema.png?raw=true)


## 5. Aprēķināt izmaksas izmantojot AWS kalkulatorus. Rezultātam jābūt bildei un aprakstam MD
Lai darbinātu lapu ir nepieciešami 2 gab. ec2 instances ((1 vcpu + 1 GB ram) - katrai). 
* Webserveris (nginx + php) 
* Datubaze (mysql)

Abiem tiek izmantots ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-20230208. 
On-demand modelī sanāk 8.47 eur/mēnesī. Zemāk vizualizācija:
![Main page](https://github.com/pavljiks/JG-MD4/blob/main/report_pictures/aws-instances.png?raw=true)
![Main page](https://github.com/pavljiks/JG-MD4/blob/main/report_pictures/aws-instance-cost.png?raw=true)


## 6. Ka varētu izskatīties Defenition of Done – šim definētiem uzdevumiem.
The Definition of Done (DoD) for a WordPress lapai varētu būt stāvoklis kas sevī iekļautu. 
1. Infrastukūra ir sagatavota ar nepieciešamām komponentēm (webserver + datu bāze). 
2. Lapas kods (wordpress kods ir izvietots uz webservera un sinhronizēts ar pašu projektu githubā). 
3. Lapas dizains un saturs ir papildināts un viegli navigējams.
4. Ir sagatavoti nepieciešamie faili (conf un izpildāmās komandas). infrastuktūras atkārtotai pacelšānai (var tikt izmantoti automatizētai izmaiņu piegādei nākotnē). 


## Additional info:

## Webserver setup 
nginx config

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


### Certificate 
**LetEncrpyt certificate** 
ACME server refuses to issue a certificate for this domain name, because it is forbidden by policy. 
```
An unexpected error occurred:
The server will not issue certificates for the identifier :: Error creating new order :: 
Cannot issue for "ec2-18-210-18-191.compute-1.amazonaws.com": 
The ACME server refuses to issue a certificate for this domain name, because it is forbidden by policy
Ask for help or search for solutions at https://community.letsencrypt.org. See the logfile /var/log/letsencrypt/letsencrypt.log or re-run Certbot with -v for more details.

```

**Self-sign certificate** 
Generate certificate (self-sign-cert.pem) and key self-sign-key.pem)
`openssl req -x509 -newkey rsa:1024 -nodes -out self-sign-cert.pem -keyout self-sign-key.pem -days 1095 -subj "/C=no/O=null/OU=null/CN=it.support@company.com"`


### Database setup 

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



