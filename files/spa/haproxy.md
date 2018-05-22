# Configuración de HAProxy para enviar la ip real

Necesitamos agregar la opción de **forwardfor** en el frontend y el funcionamiento del backend como **send-proxy**.

**Fichero de configuración de haproxy:** /etc/haproxy/haproxy.cfg

#### FRONTEND
----
Debemos agregar tanto al balanceo http como al https la siguiente opción
	option forwardfor

##### Ejemplo de frontend
###### En negrita
----
	frontend http-in
	bind XXX.XXX.XXX.XXX:80
	mode tcp
	option http-server-close
	**option forwardfor**
	default_backend http-in-backend



	frontend https-in
	bind XXX.XXX.XXX.XXX:443
	mode tcp
	**option forwardfor**
	default_backend https-in-backend

#### BACKEND
----
Habilitamos **send-proxy** en los servidores a balancear
  
##### Ejemplo de backend
###### En negrita
----  
	backend http-in-backend
	balance roundrobin
	option httpchk HEAD / HTTP/1.1\r\nHost:\ localhost
	server Nginx01 XXX.XXX.XXX.XXX:80 **send-proxy** check
	server Nginx02 YYY.YYY.YYY.YYY:80 **send-proxy** check
	server Nginx03 ZZZ.ZZZ.ZZZ.ZZZ:80 **send-proxy** check



	backend https-in-backend
	mode tcp
	balance roundrobin
	server Nginx01 XXX.XXX.XXX.XXX:443 **send-proxy** check
	server Nginx02 YYY.YYY.YYY.YYY:443 **send-proxy** check
	server Nginx03 ZZZ.ZZZ.ZZZ.ZZZ:443 **send-proxy** check


