# Wordpress

# Auto configuration parameters :

- DATABASE_HOST=localhost         ( Name of the mariadb service  )
- DATABASE_PORT=3306              ( Port of the mariadb server )
- DATABASE_PASSWORD=password      ( Password of the mariadb server )
- DATABASE_USERNAME=wordpress     ( Username of the mariadb server )
- DATABASE_NAME=wordpress         ( Name of the database on the mariadb server )
- ADMIN_USERNAME=admin            ( Wordpress admin username )
- ADMIN_PASSWORD=password         ( Wordpress admin password  )
- ADMIN_EMAIL=admin@example.org   ( Wordpress admin email )
- TRUSTED_DOMAIN=localhost        ( Wordpress trusted domain )
- SITENAME="Wordpress Site"       ( Wordpress trusted domain )

# Compose file example

```

version: '3.1'

services:

  Wordpress:
    image: dotriver/wordpress
    environment:
        - DATABASE_HOST=mariadb
        - DATABASE_PORT=3306
        - DATABASE_NAME=wordpress
        - DATABASE_USERNAME=wordpress
        - DATABASE_PASSWORD=password
        - ADMIN_USERNAME=admin
        - ADMIN_PASSWORD=password
        - TRUSTED_DOMAIN_0=172.17.0.1:8080
        - TRUSTED_DOMAIN_1=box.example.org
    ports:
      - 8080:80
    volumes:
      - wordpress-data:/var/www/wordpress/
    networks:
      default:
    deploy:
      resources:
        limits:
          memory: 256M
      restart_policy:
        condition: on-failure
      mode: global

  mariadb:
    image: dotriver/mariadb
    environment:
      - ROOT_PASSWORD=password
      - DB_0_NAME=wordpress
      - DB_0_PASS=password
    ports:
      - 3306:3306
      - 8081:80
    volumes:
      - mariadb-data:/var/lib/mysql/
      - mariadb-config:/etc/mysql/
    networks:
      default:
    deploy:
      resources:
        limits:
          memory: 256M
      restart_policy:
        condition: on-failure
      mode: global

volumes:
    wordpress-data:
    mariadb-data:
    mariadb-config:

```
