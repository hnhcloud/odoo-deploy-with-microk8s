### This is the setup guide for **odoo deploy** with microk8s
#### First you need to install microk8s on your host:

```
sudo snap install microk8s --classic --channel=1.33
sudo usermod -a -G microk8s $USER
mkdir -p ~/.kube
chmod 0700 ~/.kube
```
### You will also need to re-enter the session for the group update to take place:
```
su - $USER
microk8s status --wait-ready
```

### Access Kubernetes:
```
microk8s kubectl get node
microk8s kubectl get services
```

### Use add-on:
```
microk8s enable dns
microk8s enable hostpath-storage
microk8s enable ingress
microk8s enable cert-manager
```

### Let's create all of the yaml files that are necessary to deploy odoo with ssl certificate:

The following files that you need to create:
- config.yaml
- cluster-issuer.yaml
- odoo-deploy.yaml
- odoo-ingress.yaml
- odoo-svc.yaml
- postgres-depoly.yaml
- postgres-svc.yaml
- pv.yaml

### Now create the service with microk8s by using the yaml file:
```
microk8s kubectl create -f config.yaml
microk8s kubectl create -f pv.yaml
microk8s kubectl create -f postgres-deploy.yaml
microk8s kubectl create -f postgres-svc.yaml
microk8s kubectl create -f odoo-deploy.yaml
microk8s kubectl create -f odoo-svc.yaml
microk8s kubectl create -f cluster-issue.yaml
microk8s kubectl create -f odoo-ingress.yaml
```
### Check the certificate status:
`
microk8s kubectl get certificate
`
### Expected Console Output:

```
NAME       READY   SECRET     AGE
odoo-tls   True    odoo-tls   1m
```
### Access the odoo with your browser:
`
https://yourdomain.com
`

*Note: Before you start, You need to create DNS A Record on your domain provider (Namecheap, GoDaddy, Cloudflare, etc.):*

***Powered by Htoo Eain Lin***
