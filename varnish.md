## Varnish Cache ##

### What's Varnish? ###

> Varnish Cache is a web application accelerator also known as a caching HTTP reverse proxy. You install it in front of any server that speaks HTTP and configure it to cache the contents. Varnish Cache is really, really fast. It typically speeds up delivery with a factor of 300 - 1000x, depending on your architecture. 

### How to use Varnish in brief? ###

**Varnish** is typically a proxy hence need to combine the use of **Apache**, put Varnish in the front and set port as 80 as all broswers use this default port, and change Apache localhost port to 127.0.0.1:8080 as backend server, remember to make this change in both /usr/local/apache/conf/httpd.conf and all vhosts on your website under /usr/local/apache/conf/vhost/mysite.com.conf

Then type commands below to start:

`/etc/init.d/httpd stop`
`service varnish stop`
`service varnish start`
`/etc/init.d/httpd start`

Once Varnish is working, you need to add some plugins like **WP Super Cache** to purge cached data whenever you make changes to the site or the site won't update with your changes.

### Install and configure Varnish ##

`yum install varnish` With CentOS 7 this command will install varnish version 4 only.
`chkconfig varnish on` This will enable varnish always.

Edit config below and change the port to 80.

> vi /etc/varnish/varnish.params

> VARNISH_LISTEN_PORT=80

**/etc/varnish/default.vcl**

This file defines what the backend server is ex. apache 127.0.0.1:8080 and define the ACL for purging so that WP Super Cache can purge if needed.

Check this [config model](https://wiki.mikejung.biz/Varnish) if wanna more detailed tunning.
