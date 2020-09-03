Paquetes a instalar

Mysql 5.7

apt install mysql-client-5.7 
Php 7.3
sudo add-apt-repository ppa:ondrej/php
apt-get install -y php-apcu php-apcu-bc php-common php-psr-log php-ssh2 php7.3 php7.3-cli  php7.3-common php7.3-curl php7.3-fpm php7.3-gd php7.3-gmp php7.3-intl php7.3-json php7.3-mbstring php7.3-mysql php7.3-opcache php7.3-readline php7.3-soap php7.3-xml php7.3-zip
Nginx
-  Instalar NGINX

sudo apt-get install nginx -y 

Aws Cli

-  Instalar aws cli 
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

NodeJs
sudo su -s /bin/bash www-data
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash

Agregar en el /etc/bash.bashrc 

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm

sudo su -s /bin/bash www-data
nvm install node 
nvm --version
node --version


Composer

curl -sS https://getcomposer.org/installer |php
sudo mv composer.phar /usr/local/bin/composer
vim .bash_profile 
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion




YARN
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update && sudo apt install yarn
suwww-data
yarn add node-sass


Código fuente en S3 Valyrio

aws s3 cp s3://valyrio/www /var/www/invex-dev.aper.cloud/www --recursive 
Configuraciones 
PHP

Descomentar en /etc/php/7.3/fpm/pool.d/www
listen = 127.0.0.1:9000
NGINX
Configurar sites-available galicia-pre.aper.cloud.conf y apuntar
ln -s /etc/nginx/sites-available/galicia-pre.aper.cloud.conf /etc/nginx/sites-enabled/galicia-pre.aper.cloud.conf
Agregar ssh keys 
su ubuntu
cd .ssh
vim authorized_keys

En el archivo por default de nginx 

    location = /ping {
                include fastcgi_params;
                fastcgi_pass 127.0.0.1:9000;
        }

Descomentar las líneas en el archivo de conf del fpm para que pueda hacer este chequeo

vim /etc/php/7.3/fpm/pool.d/www.conf
ping.path = /ping
ping.response = pong
Presta
Cambiar conexion a BD en el servidor. 
/var/www/invex-dev.aper.cloud/www/app/config/parameters.php 
Base de datos
Modificar la url por la de INVEX en las tablas ps_configuration (value) y ps_shop_url (domain , domain_ssl) Cambiar en la bd 
Route53 Apernet
Apuntar invex-dev.aper.cloud al LB en route53 
Ubuntu
Activo usuario www-data para login

sed '/www-data.*/d' -i /etc/passwd
echo "www-data:x:33:33:www-data:/var/www:/bin/bash" | sudo tee -a /etc/passwd

Alias para usuario www-data

echo -e "\nalias suwww-data=\"sudo su - www-data\"" >>  /home/ubuntu/.bash_aliases
echo -e "\nalias suwww-data=\"sudo su - www-data\"" >>  /root/.bash_aliases
echo -e "\nalias suroot=\"sudo su - \"" >>  /home/ubuntu/.bash_aliases
source /root/.bash_aliases
 
Para Jenkins
 
(Estando como root)
cd /var/www
mkdir .ssh
chown www-data. .ssh
 
(Estando como www-data)
ssh-keygen -t rsa -b 4096 -C "`whoami`@`hostname`" -P "" -f "`hostname`"
cat ~/.ssh/`hostname`.pub >> ~/.ssh/authorized_keys
echo -e "User www-data\n  Hostname gitlab.com\n  IdentityFile ~/.ssh/`hostname`\n  TCPKeepAlive yes\n  IdentitiesOnly yes\n" >> ~/.ssh/config
 
Agregar la llave en gitlab en el repositorio nuevo: invex
