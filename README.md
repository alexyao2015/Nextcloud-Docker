# Nextcloud Docker Compose

Starts up Nextcloud with Redis, Mariadb, Nginx, OnlyOffice, Collabora

Nextcloud on port 8080
Onlyoffice at 8080/onlyoffice
Collabora at 8080
phpmyadmin at 8081


#Docker Compose

Make changes to docker-compose.yml using self documentation

#Reverse Proxy Usage

```bash
docker exec -u www-data nextcloud_app php occ --no-warnings config:system:set trusted_domains 2 --value="sub.example.com"
```

#Nginx Settings

Edit nginx.conf line 19 to reverse proxy address


#Onlyoffice settings

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

#Collabora Settings

URL same as FQDN

Include protocol scheme (http/https) in URL

Try including and removing http:// if not using http
Also be sure to adjust docker-compose.yml if not using http
