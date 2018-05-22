# Configuration for the Nginx side to read the original ip forwarded by HAProxy

We need to config the **balancer ip** to the nginx configuration and change the default working prototol to the **proxy_protocol**.

**Available web config file in Nginx:** etc/nginx/sites-available/*~~site~~*

At the beginning of the file, add the real balancer ip and config to work under the proxy_protocol.

>**set_real_ip_from** XXX.XXX.XXX.XXX;      **#HAProxy Balancer - Real IP**
real_ip_header **proxy_protocol**;

At virtual host area, change the default_server with **proxy_protocol**
>listen 80 **proxy_protocol**;

>listen 443 ssl **proxy_protocol**;

##### Default file (sites-available/default)  - Example File
###### In Bold
----
    #
    # Default server configuration
    #
    set_real_ip_from XXX.XXX.XXX.XXX;
    real_ip_header proxy_protocol;
    
    #HTTP
    
    server {
    	listen 80 proxy_protocol;
    	server_name deful.nginx.web;
    	rewrite ^ https://$server_name$request_uri? permanent;
    	root /var/www/default;
    }
    
    #HTTPS
    
    server {
    	gzip on
    	listen 443 ssl proxy_protocol;
    	ssl on;
    	ssl_certificate /etc/nginx/conf.d/ssl.crt;
    	ssl_certificate_key /etc/nginx/conf.d/ssl.key;
    	server_name deful.nginx.web;
    	root /var/www/default;
    
    .
    .
    .
    
    #Rest of file
    
    .
    .
    .
    }



