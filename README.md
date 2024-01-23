# Laravel project deploy in VPS server and Lemp stack setup
1. First login your VPS server.
## If you want details in this server
- **Run apt install neofetch**
- **Run  neofetch**
## Install NGINX for handle HTTP request
- **Run sudo apt update**
- **Run sudo apt install nginx**
- **Run nginx -t**
## User Firewall enable
- **Run ufw enable**
- **Run ufw status**
- **Run sudo ufw allow 'Nginx Full'**
- **Run sudo ufw allow 'OpenSSH'**
## MySQL install
- **Run sudo apt update**
- **Run sudo apt install mysql-server**
- **Run sudo mysql_secure_installation**
## Add new Database and User
- Run mysql -u root -p
- Run create database YourDatabaseName;
- Run CREATE USER 'YourUserName'@'localhost' IDENTIFIED WITH mysql_native_password BY 'YourPassword';
- Run GRANT ALL ON YourDatabaseName.* TO 'YourUserName'@'localhost';
- Run flush privileges;


## PHP Install

- **Run sudo apt update && sudo apt upgrade -y**
- **Run sudo apt install software-properties-common apt-transport-https -y**
- **Run sudo add-apt-repository ppa:ondrej/php -y**
- **Run sudo apt install php8.1-fpm php8.1-common php8.1-mysql php8.1-xml php8.1-xmlrpc php8.1-curl php8.1-gd php8.1-imagick php8.1-cli php8.1-dev php8.1-imap php8.1-mbstring php8.1-opcache php8.1-soap php8.1-zip php8.1-intl php8.1-bcmath**

## Git Install
- **Run apt install git**
## Composer Install
- **Run sudo apt install wget php-cli php-zip unzip**
- **Run cd ~**
- **Run php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"**
- **Run  HASH="$(wget -q -O - https://composer.github.io/installer.sig)"**
- **Run  php -r "if (hash_file('SHA384', 'composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"**
- **Run sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer**
- **Run composer**
## Deploy Laravel
- **Run ssh-keygen -t rsa -b 4096 -C "YourGithubMail"**
- **Run cat ~/root/.ssh/id_rsa.pub**
- **Run cd /var/www/html**
- **Run git clone ssh_project_url**
- **Run cd project**
- **Run composer install**
- **Run cp .env.example .env**
- **Run php artisan key:generate**
- **Run nano .env**
- **Run chown -R www-data:www-data storage/**
- **Run chown -R www-data:www-data bootstrap/**
- **Run chmod -R 0777 /path/to/your/project/public**
- **Run php artisan migrate**
## Server Setup
- Run cd /etc/nginx/sites-available
- Run nano default
- **Write in default file:** server {
    server_name your_ip_address_here;
    root /var/www/html/YourProjectName/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    index index.html index.htm index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
## Contributing

Contributions are welcome! Please open an issue or submit a pull request.
