# latest compose.yml
```
services:
  db:
    image: postgres:17
    restart: unless-stopped
    environment:
      POSTGRES_DB: nextcloud
      POSTGRES_USER: nextcloud
      POSTGRES_PASSWORD: Password123#
    volumes:
      - db_data:/var/lib/postgresql/data

  nextcloud:
    image: nextcloud:apache
    restart: unless-stopped
    ports:
      - "8080:80"
    environment:
      POSTGRES_HOST: db
      POSTGRES_DB: nextcloud
      POSTGRES_USER: nextcloud
      POSTGRES_PASSWORD: Password123#
    volumes:
      - nextcloud_data:/var/www/html
    depends_on:
      - db

  collabora:
    image: collabora/code
    restart: unless-stopped
    environment:
      - username=admin
      - password=Password123#
      - server_name=192.168.178.25:9980
      - aliasgroup1=http://nextcloud:80
      - domain=nextcloud
      - extra_params=--o:ssl.enable=false --o:ssl.termination=false

    ports:
      - "9980:9980"
    cap_add:
      - MKNOD

volumes:
  db_data:
  nextcloud_data:
```
# explained:

server_name = `192.168.178.25:9980`  
"How users/browser reach Collabora"


domain = `nextcloud`  
"Which WOPI host is allowed"


aliasgroup1 = `http://nextcloud:80`  
"How Collabora reaches Nextcloud"









