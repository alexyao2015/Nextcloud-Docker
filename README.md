# Nextcloud Docker Compose

Starts up Nextcloud with Redis, Mariadb, Nginx, OnlyOffice

```bash
docker exec -u www-data nextcloud_app php occ --no-warnings config:system:set allow_local_remote_servers --value=true
docker exec -u www-data nextcloud_app php occ --no-warnings config:system:set trusted_domains 1 --value="web"
```

#Onlyoffice settings:

## Document Editing Service address
`/onlyoffice/`

## Document Editing Service address for internal requests from the server
`http://onlyoffice/`

## Server address for internal requests from the Document Editing Service
`http://web/`
