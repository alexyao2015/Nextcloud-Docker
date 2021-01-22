# Nextcloud Docker Compose

Starts up Nextcloud with Redis, Mariadb, Nginx, OnlyOffice

#Docker Compose:

Make changes to docker-compose.yml using self documentation

#Reverse Proxy usage:

```bash
docker exec -u www-data nextcloud_app php occ --no-warnings config:system:set trusted_domains 2 --value="sub.example.com"
```

#Nginx settings:

Edit nginx.conf line 19 to reverse proxy address


#Onlyoffice settings:

```bash
docker exec -u www-data nextcloud_app php occ --no-warnings config:system:set allow_local_remote_servers --value=true
docker exec -u www-data nextcloud_app php occ --no-warnings config:system:set trusted_domains 1 --value="web"
```

## Document Editing Service address

`/onlyoffice/`

## Document Editing Service address for internal requests from the server

`http://onlyoffice/`

## Server address for internal requests from the Document Editing Service

`http://web/`
