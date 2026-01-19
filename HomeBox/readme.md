Home Box





```

services:

&nbsp; homebox:

&nbsp;   container\_name: Homebox

&nbsp;   image: ghcr.io/sysadminsmedia/homebox:latest

&nbsp;   mem\_limit: 3g

&nbsp;   cpu\_shares: 1024

&nbsp;   security\_opt:

&nbsp;     - no-new-privileges:true

&nbsp;   ports:

&nbsp;     - 3100:7745

&nbsp;   volumes:

&nbsp;     - /volume1/docker/homebox:/data:rw

&nbsp;   environment:

&nbsp;      HBOX\_LOG\_LEVEL: info

&nbsp;      HBOX\_LOG\_FORMAT: text

&nbsp;      HBOX\_WEB\_MAX\_UPLOAD\_SIZE: 5000

&nbsp;      TZ: America/Toronto

&nbsp;   restart: on-failure:5

```

