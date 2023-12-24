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

## install k3s 

```
curl -sfL https://get.k3s.io | sh -
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


