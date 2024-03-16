## install docker


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


## Install Cloudflare tunnel (cloudflared)
Use the cloudflare docker-compose
and the `environment_variables.sh`
Add the file to `~/.bashrc` with `source /home/scoop/NAS/vallhalla/Variables_entorno.sh`

##Install portainer 
`docker run -d -p 9000:9000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest`



