# HAProxy configs to forward the real ip

We need to add the **forwardfor** to the frontend and the **send-proxy** protocol to the backend.

**HAProxy config file:** /etc/haproxy/haproxy.cfg

#### FRONTEND
----
To the http and the https frontend configuration, we add the option:
	option forwardfor

##### frontend example
----
	frontend http-in
	bind XXX.XXX.XXX.XXX:80
	mode tcp
	option http-server-close
	option forwardfor
	default_backend http-in-backend


	frontend https-in
	bind XXX.XXX.XXX.XXX:443
	mode tcp
	option forwardfor
	default_backend https-in-backend

#### BACKEND
----
Enable the **send-proxy** to the balanced server
  
##### backend example
----  
	backend http-in-backend
	balance roundrobin
	option httpchk HEAD / HTTP/1.1\r\nHost:\ localhost
	server Nginx01 XXX.XXX.XXX.XXX:80 send-proxy check
	server Nginx02 YYY.YYY.YYY.YYY:80 send-proxy check
	server Nginx03 ZZZ.ZZZ.ZZZ.ZZZ:80 send-proxy check



	backend https-in-backend
	mode tcp
	balance roundrobin
	server Nginx01 XXX.XXX.XXX.XXX:443 send-proxy check
	server Nginx02 YYY.YYY.YYY.YYY:443 send-proxy check
	server Nginx03 ZZZ.ZZZ.ZZZ.ZZZ:443 send-proxy check


