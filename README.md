# k3s_single_node_ubuntu
Installation of a k3s single node in ubuntu for a home server/ home lab


## prepare ubuntu

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install -y grep curl
sudo apt-get install -y openssh-server openssh-client
```

And configure the IP if necessary.

## install k3s without traefik (we will install NGINX)

```
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="server --disable=traefik" sh -s -
```

After installation check is running 

```
sudo systemctl status k3s
```

Give permissions
```
export KUBECONFIG=~/.kube/config

mkdir ~/.kube 2> /dev/null
sudo k3s kubectl config view --raw > "$KUBECONFIG"
chmod 600 "$KUBECONFIG"
```
You can add `KUBECONFIG=~/.kube/config` to your ~/.profile or ~/.bashrc to make it persist on reboot.
better if you do on bashrc


Check this to ensure you are able to work

```
kubectl get nodes
kubectl cluster-info
kubectl get pods -A
```



## configure volume drives from NAS 
We will add a shared drive to copy the volumes that we will use to ensure to not lose any data.
```
sudo apt-get install cifs-utils
or
sudo apt-get install nfs-common

mkdir ~/share
cd ~/share

FOR SMB MOUNT
sudo mount -t cifs //192.168.20.100/HomeNAS ~/share/ -o username=scoop96
or
FOR NFS MOUNT
sudo mount -t nfs 192.168.20.100:/mnt/SSD_2TB/docker-volumes ~/share/

df -h
sudo umount ~/share/

```

To automate the mount after reboot we should add this line to `/etc/fstab` 

```
//truenas.local/HomeNAS /home/scoop/share/  cifs  username=scoop96,password=XXXXXXX,noauto,x-systemd.automount 0 0
```

and use these commands
```

sudo systemctl daemon-reload
sudo systemctl restart remote-fs.target

noauto == makes it so it will not mount automatically at boot
x-systemd.automount == makes it so it will mount on demand
```


