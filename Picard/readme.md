



```

version: "3.8"



services:

&nbsp; musicbrainz-picard:

&nbsp;   image: jlesage/musicbrainz-picard

&nbsp;   container\_name: musicbrainz-picard

&nbsp;   restart: unless-stopped

&nbsp;   ports:

&nbsp;     - "5800:5800"   # Browser GUI

&nbsp;     - "5900:5900"   # Optional VNC

&nbsp;   volumes:

&nbsp;     - /mnt/nas/source\_music:/source:rw

&nbsp;     - /mnt/nas/target\_library:/library:rw

&nbsp;     - /mnt/nas/picard\_config:/config:rw

&nbsp;   environment:

&nbsp;     - TZ=America/Toronto



```

