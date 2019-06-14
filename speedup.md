There are 4 keys to speed up your website:

1. Good VPS, good hardware, that's the first point
2. Adjust your servers including Apache + MySQL + PHP + add BBR to OS
3. Open OPcache of PHP, try memcached + varnish for big site
4. CDN, this can help to secure and speedup a lot
5. Optimizing the Front-end, here this article we will focus on this subject


## Optimizing front end ##

Front end is normally the barriers for loading time, there are a lot tricks. Check [GTmetrix.com](https://gtmetrix.com) and see how your site's performance and optimizing suggestions.

### Add expires headers ###

> Expires headers let the browser know whether to serve a cached version of the page.

* Reduce server load
* Decrease page load time
* Cost benefit ration: high value
* Access needed

For more details, pls check [GTmetrix: YSlow: Add Expires headers](https://gtmetrix.com/add-expires-headers.html)

In general, add following lines into the **.htaccess** which locate in your web root.

```
<IfModule mod_expires.c>
  ExpiresActive On

  # Images
  ExpiresByType image/jpeg "access plus 1 year"
  ExpiresByType image/gif "access plus 1 year"
  ExpiresByType image/png "access plus 1 year"
  ExpiresByType image/webp "access plus 1 year"
  ExpiresByType image/svg+xml "access plus 1 year"
  ExpiresByType image/x-icon "access plus 1 year"

  # Video
  ExpiresByType video/mp4 "access plus 1 year"
  ExpiresByType video/mpeg "access plus 1 year"

  # CSS, JavaScript
  ExpiresByType text/css "access plus 1 month"
  ExpiresByType text/javascript "access plus 1 month"
  ExpiresByType application/javascript "access plus 1 month"

  # Others
  ExpiresByType application/pdf "access plus 1 month"
  ExpiresByType application/x-shockwave-flash "access plus 1 month"
</IfModule>
```

*Depending on your website's files you can set different expiry times. If certain types of files are updated more frequently, you would set an earlier expiry time on them (ie. css files)*
