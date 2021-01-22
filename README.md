# Nextcloud Docker Compose

Starts up Nextcloud with Redis, Mariadb, Nginx, OnlyOffice, Collabora

Nextcloud on port 8080
Onlyoffice at 8080/onlyoffice
Collabora at 8080
phpmyadmin at 8081

# Setup
## Docker Compose

Make changes to docker-compose.yml using self documentation

## Reverse Proxy Usage

```bash
docker exec -u www-data nextcloud_app php occ --no-warnings config:system:set trusted_domains 2 --value="sub.example.com"
```

## Nginx Settings

Edit nginx.conf line 19 to reverse proxy address


## Onlyoffice settings

Onlyoffice may take some time to startup before it will become active

```bash
docker exec -u www-data nextcloud_app php occ --no-warnings config:system:set allow_local_remote_servers --value=true
docker exec -u www-data nextcloud_app php occ --no-warnings config:system:set trusted_domains 1 --value="web"
```

### Document Editing Service address

`/onlyoffice/`

### Document Editing Service address for internal requests from the server

`http://onlyoffice/`

### Server address for internal requests from the Document Editing Service

`http://web/`

## Collabora Settings

URL same as FQDN

Include protocol scheme (http/https) in URL

Try including and removing http:// if not using http
Also be sure to adjust docker-compose.yml if not using http


# Backup and Restore

All volumes denoted with `-v` and networks `--network` are prefixed by the parent folder name.
Find the prefix by checking the name or looking in `docker volume` and `docker network`


Better naming : \`date +"%Y%m%d"\`


## Nextcloud

Change 
`nextcloud_nextcloud` -> `volume`
`backup.tar.gz` -> `backup name`

### Backup

```bash
docker exec -u www-data nextcloud_app php occ maintenance:mode --on
docker run --rm -v nextcloud_nextcloud:/data -v ${PWD}:/backup busybox tar zcvf /backup/backup.tar.gz /data > /dev/null 2>&1
docker exec -u www-data nextcloud_app php occ maintenance:mode --off
```

### Restore

```bash
docker exec -u www-data nextcloud_app php occ maintenance:mode --on
docker run --rm -v nextcloud_nextcloud:/data -v ${PWD}:/backup alpine sh -c "rm -rf /data/* ; tar zxvf /backup/backup.tar.gz --strip 1 -C /data;"
docker exec -u www-data nextcloud_app php occ maintenance:mode --off
```

## DB

TODO: Build alpine with mariadb-client instead of installing each time

Username and password defaults to `nextcloud` and `example`

Change 
`nextcloud_default` -> `network`
`nextcloud-sqlbkp.bak` -> `backup name`

### Backup
```bash
docker exec -u www-data nextcloud_app php occ maintenance:mode --on
docker run --rm --network nextcloud_default -v ${PWD}:/backup alpine sh -c "apk add mariadb-client ; mysqldump --single-transaction -h db -u [username] -p[password] nextcloud > /backup/nextcloud-sqlbkp.bak;"
docker exec -u www-data nextcloud_app php occ maintenance:mode --off
```

### Restore
```bash
docker exec -u www-data nextcloud_app php occ maintenance:mode --on
docker run --rm --network nextcloud_default -v ${PWD}:/backup alpine sh -c "apk add mariadb-client ; mysql -h db -u nextcloud -pexample -e \"DROP DATABASE nextcloud\" ; mysql -h db -u nextcloud -pexample -e \"CREATE DATABASE nextcloud\" ; mysql -h db -u [username] -p[password] nextcloud < /backup/nextcloud-sqlbkp.bak;"
docker exec -u www-data nextcloud_app php occ maintenance:mode --off
```
