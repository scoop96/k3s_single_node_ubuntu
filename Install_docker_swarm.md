`sudo rsync -av /mnt/HDD2TB/docker/volumes/nextcloud/ /home/scoop/share/volumes/nextcloud/`

## install docker
```
sudo crontab -e 
0 3 * * * rsync -a --delete /mnt/HDD2TB/docker/volumes/nextcloud/ /home/scoop/share/volumes/nextcloud/
crontab -e
*/15 * * * * docker exec -u 33 -it nextcloud ./occ preview:pre-generate
*/6 * * * *  timeout 1m docker exec -u 33 nextcloud php -f /var/www/html/cron.php
```

`https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository`

```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```


to create the docker group and add your user:

Create the docker group.
`sudo groupadd docker`
Add your user to the docker group.
`sudo usermod -aG docker ${USER}`
You would need to loog out and log back in so that your group membership is re-evaluated or type the following command:
`su -s ${USER}`

Logout and login to use docker without `sudo`


https://docs.docker.com/engine/install/linux-postinstall/

Enable service 

```
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```
## create networks

## Install Cloudflare tunnel (cloudflared)
Use the cloudflare docker-compose
and the `environment_variables.sh`
Add the file to `~/.bashrc` with `source /home/scoop/NAS/vallhalla/Variables_entorno.sh`

##Install portainer 
`docker run -d -p 9000:9000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest` 

add portainer to `frontend` network


##Install nextcloud
https://www.youtube.com/watch?v=iFHbzWhKfuU&t=119s

https://dbt3ch.com/books/nextcloud-with-cloudflare-tunnels/page/nextcloud-with-cloudflare-tunnels
https://www.youtube.com/watch?v=p0I8pikm2P4
Before launching the docker compose you need to create or modify the paths for the volumes.
```
    - "/path/to/nextcloud:/var/www/html"
    - "/path/to/apps:/var/www/html/custom_apps"
    - "/path/to/config:/var/www/html/config"
    - "/path/to/data:/var/www/html/data"
    - "/path/to/theme:/var/www/html/themes/<YOUR_CUSTOM_THEME>"
```

Launch the `docker-compose` from nextcloud folder.

Get inside the nextcloud bash 
`docker exec -it nextcloud bash`

```
apt update
apt install nano
```
Inside the `.htaccess`  add
`nano .htaccess`
```


php_value upload_max_filesize 16G
php_value post_max_size 16G
php_value max_input_time 3600
php_value max_execution_time 3600
php_value memory_limit 2048M



<IfModule mod_rewrite.c>
  RewriteEngine on
  RewriteRule ^\.well-known/carddav https://nextcloud.domain.com/remote.php/dav [R=301,L]
  RewriteRule ^\.well-known/caldav https://nextcloud.domain.com/remote.php/dav [R=301,L]
  RewriteRule ^\.well-known/webfinger https://nextcloud.domain.com/index.php/.well-known/webfinger [R=301,L]
  RewriteRule ^\.well-known/nodeinfo https://nextcloud.domain.com/index.php/.well-known/nodeinfo [R=301,L]
</IfModule>
<IfModule mod_headers.c>
    Header always set Strict-Transport-Security "max-age=15552000; includeSubDomains"
</IfModule>
```

HACER LA INSTALACION DEL SMBCLIENT

```
apt update

apt install smbclient
```


Now edit the `config/config.php` file with nano and add these lines

```
'trusted_domains' => 
  array (
    0 => '192.168.1.65:8080',
),
'overwriteprotocol' => 'https',
'default_phone_region' => 'ES',
'loglevel' => 2,
'maintenance_window_start' => 1,
```
```

*/6 * * * * timeout 1m docker exec -u 33 nextcloud php -f /var/www/html/cron.php 

```
`chmod -R 777 /ruta/a/la/carpeta`
`docker exec -u 33 -it nextcloud php occ db:add-missing-indices`
 
https://hijosdeinit.gitlab.io/howto_solucionando_errores_bdd_NextCloud_comando_occ/




#after so you need to install kuma for nextcloud 



simply launch the docker-compose inside the `uptime_kuma/`
Remmember to modify the cron in nextcloud.
and then we add a new monitor in kuma with the endpoint `<nextcloud endpoint>/cron.php` at 180 secs and thats it




##memories
docker exec -u 33 -it nextcloud  ./occ preview:generate-all
cronjob each 15 mins in `crontab -e`
docker exec -u 33 -it nextcloud  ./occ preview:generate



docker exec -it nextcloud bash -c "apt update && apt -y install ffmpeg"


