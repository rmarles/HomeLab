version: "3.8"

services:
  netbox-db:
    image: postgres:15-alpine
    container_name: netbox-db
    restart: unless-stopped
    environment:
      POSTGRES_DB: netbox
      POSTGRES_USER: netbox
      POSTGRES_PASSWORD: ChangePassword1
    volumes:
      - netbox-postgres:/var/lib/postgresql/data
    networks:
      - oxidized-librenms_default

  netbox-redis:
    image: redis:7-alpine
    container_name: netbox-redis
    restart: unless-stopped
    volumes:
      - netbox-redis:/data
    networks:
      - oxidized-librenms_default

  netbox:
    image: lscr.io/linuxserver/netbox:latest
    container_name: netbox
    restart: unless-stopped
    depends_on:
      - netbox-db
      - netbox-redis
    environment:
      PUID: 1000
      PGID: 1000
      TZ: America/Toronto

      DB_HOST: netbox-db
      DB_NAME: netbox
      DB_USER: netbox
      DB_PASSWORD: ChangePassword1

      REDIS_HOST: netbox-redis

      SUPERUSER_NAME: admin
      SUPERUSER_EMAIL: email@domain.com
      SUPERUSER_PASSWORD: ChangePassword2

      ALLOWED_HOST: "*"
    ports:
      - "8100:8000"
    volumes:
      - netbox-config:/config
    networks:
      - oxidized-librenms_default

volumes:
  netbox-postgres:
  netbox-redis:
  netbox-config:

networks:
  oxidized-librenms_default:
    external: true
