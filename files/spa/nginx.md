# Configuración del servidor Nginx para recibir la ip reenviada por HAProxy

Necesitamos configurar la **ip del balanceador** en el servidor nginx y configurar el servidor para funcionar en **proxy-protocol**.

**Fichero de configuración del sitio web en Nginx:** etc/nginx/sites-available/*~~site~~*

Al inicio del fichero configuramos la ip que entrega nuestro balanceador e indicamos que desde esa ip recibiremos tráfico bajo proxy_protocol.

	set_real_ip_from XXX.XXX.XXX.XXX;      #IP del balanceador HAProxy
	real_ip_header proxy_protocol;

En el apartado de configuración de los virtual host, cambiamos default_server por **proxy_protocol**


	listen 80 proxy_protocol;
	listen 443 ssl proxy_protocol;

##### Fichero default (sites-available/default) de ejemplo
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



