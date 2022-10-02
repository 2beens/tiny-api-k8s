## Tiny API with Ingress

In order to use this, ingress controller has to be added first (via helm):

```sh
# first need to add Nginx Ingress Controller repository to Helm by running:
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

# update to let Helm know what it contains:
helm repo update

# finally, run the following command to install the Nginx ingress:
helm install nginx-ingress ingress-nginx/ingress-nginx --set controller.publishService.enabled=true
```

More info can be found in a small DigitalOcean tutorial: [How To Set Up an Nginx Ingress on DigitalOcean Kubernetes Using Helm](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nginx-ingress-on-digitalocean-kubernetes-using-helm)
