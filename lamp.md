## Get Server ready before installing Wordpress ##

*Compare two http servers: Apache is good at stability, Nginx is famous for perfance especially high pressure, here Apache is chosen which is good for small business site.*

There are several LAMP pack that could easy the installation job, [Teddysun's lamp](https://github.com/teddysun/lamp) on GitHub seems a fair choice, **free Let's Encrypt is included too.**

> LAMP is a powerful bash script for the installation of Apache + PHP + MySQL/MariaDB/Percona Server and so on. You can install Apache + PHP + MySQL/MariaDB/Percona Server in an very easy way, just need to choose what you want to install before installation. And all things will be done in few minutes.

### Installation ###

```
yum -y install wget screen git
git clone https://github.com/teddysun/lamp.git
cd lamp
chmod 755 *.sh
screen -S lamp
./lamp.sh
```

During the installing process, some decisions you should make, like the version  and model you like. Normally higher version means better performance especially to PHP, hereby is the recommendation:

```
MySQL: 5.7
PHP: 7.4
Apache: 2.4.39
```

PHP additional extensions to pick

```
ImageMagick # for zooming/croping pictures
memcached # an excellent cache
```

## Configure for better performance ##

No matter MySQL or Apache, PHP all need some adjustments to get better performance.


