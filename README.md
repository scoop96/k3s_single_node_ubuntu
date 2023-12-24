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

Check this to ensure you are able to work

```
kubectl get nodes
kubectl cluster-info
kubectl get pods -A
```
