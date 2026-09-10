```
services:
  db:
    image: postgres:17
    restart: unless-stopped
    environment:
      POSTGRES_DB: nextcloud
      POSTGRES_USER: nextcloud
      POSTGRES_PASSWORD: CHANGE_THIS_DB_PASSWORD
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
      POSTGRES_PASSWORD: CHANGE_THIS_DB_PASSWORD
    volumes:
      - nextcloud_data:/var/www/html
    depends_on:
      - db

volumes:
  db_data:
  nextcloud_data:
```
```
docker compose config
```


