Before you can twist anything, point your domain like example.com to your IP address, in order to do this, you will need to login your domain dashboard, update **DNS Record** and add two following lines:

HOSTNAME | TYPE | ADDRESS/VALUE | DISTANCE/PRIO | TTL
-------- | ---- | ------------- | ------------- | ---
@ | A | 192.168.0.1 | NA | 17027
WWW | A | 192.168.0.1 | NA | 17027

## Prepare for installing Wordpress ##

1. Make sure port 80 and port 443 are enabled, to make this you will need to shutdown your firewall (ufw/firewalld/iptables) or configure the firewall.

```
systemctl start firewalld.service  # Launch FirewallD service
firewall-cmd --zone=public --add-port=80/tcp --permanent  # Open port 80
firewall-cmd --zone=public --add-port=443/tcp --permanent  # Open port 443
firewall-cmd --reload  # Active new added rules
```

```
ufw allow 80/tcp  # Open port 80
ufw allow 443/tcp  # Open port 443
systemctl ufw restart  ## Active new rules
```

```
/sbin/iptables -I INPUT -p tcp --dport 80 -j ACCEPT
/sbin/iptables -I INPUT -p tcp --dport 443 -j ACCEPT
/etc/rc.d/init.d/iptables save
/etc/init.d/iptables restart
```

If it's hosted on Aliyun, make sure to build similar security rules on your dashboard.

## Creating Virtual Host ##

Once login your VPS, move to lamp directoy by type `cd lamp`, then start to create a virtual host for the site.

`lamp add`

Above command will lead you through the whole process, when asking for the domain, be sure to enter both **example.com www.example.com**, then there is nothing special, choose Lets Encrypt to get a free SSL certificate. Once done, your are good to have a cup of tea.

** Be sure to remember your database name and password **

## Download Wordpress ##

```
cd
yum install wget  # Install a downloading tool
wget https://wordpress.org/latest.tar.gz
tar -zxvf latest.tar.gz  # Decompress files
mv ./wordpress/* /data/www/example.com  # Replace example.com to your domain
chown -R apache:apache /data/www/example.com
find /data/www/example.com/ -type d -exec chmod 755 {} \;
find /data/www/example.com/ -type f -exec chmod 644 {} \;  # Set up the secure permission
```

## Start install Wordpress ##

Open browser and type your domain like example.com while will lead you to a installation page, some information here needs you to offer including:

1. Database name
2. Database user name
3. Database password
4. [Prefix] change default "wp_" to anyting others like "wp1_" to better secure your database

## Configure Wordpress ##

1. Change [Settings] -> [Permalinks]
2. Edit wp-config.php to add following two lines to active cache

```
define('ENABLE_CACHE', true);
define('WP_CACHE', true);
```





