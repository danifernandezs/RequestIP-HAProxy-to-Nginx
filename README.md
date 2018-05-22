# IP real de la request (Del cliente) desde HAProxy (Como balanceador de carga) hacia Nginx

Notas rápidas para habilitar el reenvio de la ip original/real del cliente al hacer una request que es entregada por HAProxy hacia uno o varios nginx.

Se configura el forward-for y send-proxy del lado HAProxy
Se configura la ip del balanceador y el proxy-protocol del lado nginx

# Real Request IP (Client) From HAProxy(as load balancer) to Nginx

Quick notes to enable the original/real ip forwarding when a client make a request to the HAProxy and balanced to one or several nginx.

From the HAProxy side, enable the forward-for and the send-proxy
From nginx side, add the balancer ip and enable the proxy-protocol

## LICENCIA/LICENSE
![License Logo](./img/license/CC4-by-sa.png)

Todo el repositorio queda bajo licencia [Creative Commons Attribution-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-sa/4.0/deed.es_ES).

Se puede revisar el fichero de licencia para más información.



All repo are under [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

You can see the License file for more information.
