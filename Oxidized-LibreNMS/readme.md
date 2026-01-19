Oxidized + LibreNMS



The most difficult so far!



```

services:

&nbsp; db:

&nbsp;   image: mariadb:10

&nbsp;   container\_name: librenms\_db

&nbsp;   command:

&nbsp;     - "mysqld"

&nbsp;     - "--innodb-file-per-table=1"

&nbsp;     - "--lower-case-table-names=0"

&nbsp;     - "--character-set-server=utf8mb4"

&nbsp;     - "--collation-server=utf8mb4\_unicode\_ci"

&nbsp;   volumes:

&nbsp;     - db-data:/var/lib/mysql

&nbsp;   environment:

&nbsp;     - "TZ=${TZ}"

&nbsp;     - "MYSQL\_ROOT\_PASSWORD=${MYSQL\_ROOT\_PASSWORD}"

&nbsp;     - "MYSQL\_DATABASE=${MYSQL\_DATABASE}"

&nbsp;     - "MYSQL\_USER=${MYSQL\_USER}"

&nbsp;     - "MYSQL\_PASSWORD=${MYSQL\_PASSWORD}"

&nbsp;   restart: always



&nbsp; redis:

&nbsp;   image: redis:7.2-alpine

&nbsp;   container\_name: librenms\_redis

&nbsp;   environment:

&nbsp;     - "TZ=${TZ}"

&nbsp;   restart: always



&nbsp; msmtpd:

&nbsp;   image: crazymax/msmtpd:latest

&nbsp;   container\_name: librenms\_msmtpd

&nbsp;   env\_file:

&nbsp;     - "stack.env"

&nbsp;   restart: always



&nbsp; librenms:

&nbsp;   image: librenms/librenms:latest

&nbsp;   container\_name: librenms

&nbsp;   hostname: librenms

&nbsp;   cap\_add:

&nbsp;     - NET\_ADMIN

&nbsp;     - NET\_RAW

&nbsp;   ports:

&nbsp;     - 3300:8000

&nbsp;   depends\_on:

&nbsp;     - db

&nbsp;     - redis

&nbsp;     - msmtpd

&nbsp;   volumes:

&nbsp;     - app-data:/data

&nbsp;   env\_file:

&nbsp;     - "stack.env"

&nbsp;   environment:

&nbsp;     - "TZ=${TZ}"

&nbsp;     - "PUID=${PUID}"

&nbsp;     - "PGID=${PGID}"

&nbsp;     - "DB\_HOST=db"

&nbsp;     - "DB\_NAME=${MYSQL\_DATABASE}"

&nbsp;     - "DB\_USER=${MYSQL\_USER}"

&nbsp;     - "DB\_PASSWORD=${MYSQL\_PASSWORD}"

&nbsp;     - "DB\_TIMEOUT=60"

&nbsp;   restart: always



&nbsp; dispatcher:

&nbsp;   image: librenms/librenms:latest

&nbsp;   container\_name: librenms\_dispatcher

&nbsp;   hostname: librenms-dispatcher

&nbsp;   cap\_add:

&nbsp;     - NET\_ADMIN

&nbsp;     - NET\_RAW

&nbsp;   depends\_on:

&nbsp;     - librenms

&nbsp;     - redis

&nbsp;   volumes:

&nbsp;     - app-data:/data

&nbsp;   env\_file:

&nbsp;     - "stack.env"

&nbsp;   environment:

&nbsp;     - "TZ=${TZ}"

&nbsp;     - "PUID=${PUID}"

&nbsp;     - "PGID=${PGID}"

&nbsp;     - "DB\_HOST=db"

&nbsp;     - "DB\_NAME=${MYSQL\_DATABASE}"

&nbsp;     - "DB\_USER=${MYSQL\_USER}"

&nbsp;     - "DB\_PASSWORD=${MYSQL\_PASSWORD}"

&nbsp;     - "DB\_TIMEOUT=60"

&nbsp;     - "DISPATCHER\_NODE\_ID=dispatcher1"

&nbsp;     - "SIDECAR\_DISPATCHER=1"

&nbsp;   restart: always



&nbsp; syslogng:

&nbsp;   image: librenms/librenms:latest

&nbsp;   container\_name: librenms\_syslogng

&nbsp;   hostname: librenms-syslogng

&nbsp;   cap\_add:

&nbsp;     - NET\_ADMIN

&nbsp;     - NET\_RAW

&nbsp;   depends\_on:

&nbsp;     - librenms

&nbsp;     - redis

&nbsp;   ports:

&nbsp;     - 3301:514/tcp

&nbsp;     - 3301:514/udp

&nbsp;   volumes:

&nbsp;     - app-data:/data

&nbsp;   env\_file:

&nbsp;     - "stack.env"

&nbsp;   environment:

&nbsp;     - "TZ=${TZ}"

&nbsp;     - "PUID=${PUID}"

&nbsp;     - "PGID=${PGID}"

&nbsp;     - "DB\_HOST=db"

&nbsp;     - "DB\_NAME=${MYSQL\_DATABASE}"

&nbsp;     - "DB\_USER=${MYSQL\_USER}"

&nbsp;     - "DB\_PASSWORD=${MYSQL\_PASSWORD}"

&nbsp;     - "DB\_TIMEOUT=60"

&nbsp;     - "SIDECAR\_SYSLOGNG=1"

&nbsp;   restart: always



&nbsp; snmptrapd:

&nbsp;   image: librenms/librenms:latest

&nbsp;   container\_name: librenms\_snmptrapd

&nbsp;   hostname: librenms-snmptrapd

&nbsp;   cap\_add:

&nbsp;     - NET\_ADMIN

&nbsp;     - NET\_RAW

&nbsp;   depends\_on:

&nbsp;     - librenms

&nbsp;     - redis

&nbsp;   ports:

&nbsp;     - 3302:162/tcp

&nbsp;     - 3302:162/udp

&nbsp;   volumes:

&nbsp;     - app-data:/data

&nbsp;   env\_file:

&nbsp;     - "stack.env"

&nbsp;   environment:

&nbsp;     - "TZ=${TZ}"

&nbsp;     - "PUID=${PUID}"

&nbsp;     - "PGID=${PGID}"

&nbsp;     - "DB\_HOST=db"

&nbsp;     - "DB\_NAME=${MYSQL\_DATABASE}"

&nbsp;     - "DB\_USER=${MYSQL\_USER}"

&nbsp;     - "DB\_PASSWORD=${MYSQL\_PASSWORD}"

&nbsp;     - "DB\_TIMEOUT=60"

&nbsp;     - "SIDECAR\_SNMPTRAPD=1"

&nbsp;   restart: always



&nbsp; oxidized:

&nbsp;   image: oxidized/oxidized:latest

&nbsp;   container\_name: librenms\_oxidized

&nbsp;   environment:

&nbsp;     CONFIG\_RELOAD\_INTERVAL: 600

&nbsp;   volumes:

&nbsp;      - oxidized-config:/home/oxidized/.config/oxidized/

&nbsp;   restart: always



volumes:

&nbsp; db-data:

&nbsp; app-data:

&nbsp; oxidized-config:

``




Phase 1 – LibreNMS + Oxidized (foundation)

Goal: Get configs backed up reliably before automation spreads.

Configure SNMP + SSH on ProCurve

Add ProCurve to LibreNMS

Configure Oxidized

LibreNMS as source

SSH credentials

Enable HP/ProCurve model

Verify:

show run pulled

Stored locally

Add GitHub repo

SSH key or token

Push configs per-device

👉 This gives you:

Monitoring

Config history

Safe rollback point

This should be the first new chat you start.