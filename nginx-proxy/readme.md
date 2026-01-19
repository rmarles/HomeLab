nginx Proxy



```

version: "3.8"



services:

&nbsp; nginx:

&nbsp;   image: nginx:alpine

&nbsp;   container\_name: nginx\_proxy

&nbsp;   restart: always

&nbsp;   ports:

&nbsp;     - "80:80"

&nbsp;     - "443:443"

&nbsp;   volumes:

&nbsp;     - /srv/nginx/conf.d:/etc/nginx/conf.d:ro

&nbsp;     - /srv/nginx/certs:/etc/nginx/certs:ro

&nbsp;     - /srv/nginx/html:/usr/share/nginx/html

&nbsp;   networks:

&nbsp;     - nginx-proxy\_nginx\_proxy\_net



networks:

&nbsp; nginx-proxy\_nginx\_proxy\_net:

&nbsp;   external: true

```

