## Get Server ready before installing Wordpress ##

*Compare two http servers: Apache is good at stability, Nginx is famous for perfance especially high pressure, here Apache is chosen which is good for small business site.*

There are several LAMP pack that could easy the installation job, [Teddysun's lamp](https://github.com/teddysun/lamp) on GitHub seems a fair choice, **free SSL Let's Encrypt is included too.**

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

### Adjust MySQL ###

Edit the configuration file
`vi /etc/my.cnf`

```
default-storage-engine = MyISAM # Lower memory usage than InnoDB if 512M RAM available only
myisam_sort_buffer_size= 64M
query_cache_size       = 64M
query_cache_type       = 1

```
For security cause, add following line into [mysqld] sector of /etc/my.cnf

`bind-address = 127.0.0.1`

Thus MySQL will be only visited by local not remotely.



### Adjust Apache ###

1. Enable Gzip
Uncomment the following two lines in /usr/local/apache/conf/httpd.conf

>LoadModule deflate_module modules/mod_deflate.so
>LoadModule headers_module modules/mod_headers.so

Then add the following configuration to the end of httpd.conf

```
<ifmodule mod_deflate.c>
SetOutputFilter DEFLATE
SetEnvIfNoCase Request_URI .(?:gif|jpe?g|png)$ no-gzip dont-vary
SetEnvIfNoCase Request_URI .(?:exe|t?gz|zip|bz2|sit|rar)$ no-gzip dont-vary 
SetEnvIfNoCase Request_URI .(?:pdf|mov|avi|mp3|mp4|rm)$ no-gzip dont-vary
DeflateCompressionLevel 6
AddOutputFilterByType DEFLATE text/html text/plain text/xml application/x-httpd-php
AddOutputFilter DEFLATE js css
</ifmodule>
```

2. Set httpd-mpm.conf in /usr/local/apache/conf/extra

Firstly uncomment `Include conf/extra/httpd-mpm.conf` in /usr/local/apache/conf/httpd.conf

```
<IfModule mpm_event_module>
    StartServers             3
    MinSpareThreads         35
    MaxSpareThreads         85
    ThreadsPerChild         25
    MaxRequestWorkers      400
    MaxConnectionsPerChild   50
</IfModule>
```

You will see many mpm available, the default mpm is event. 
Default value of **MaxConnectionPerChild** is 0 which means **unlimited**, change this or Apache will cost a lot of memory and the hence a "Kill" of Apache happened, that means your website will be down untill enough memeory available again.

3. Enable `httpd-default.conf

Uncomment `Include conf/extra/httpd-default.conf` in /usr/loca/apache/conf/httpd.conf
