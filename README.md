# Tiny API with Kubernetes experimentations
k8s config files for the projects:
 - [tiny-api](https://github.com/2beens/tiny-api)
 - [tiny-api-web-client](https://github.com/2beens/tiny-api-web-client)
 - [tiny-service](https://github.com/2beens/tiny-service)

Related docker images:
 - [docker/stubin87/tiny-api](https://hub.docker.com/repository/docker/stubin87/tiny-api)
 - [docker/stubin87/tiny-api-web-client](https://hub.docker.com/repository/docker/stubin87/tiny-api-web-client)
 - [stubin87/tiny-service](https://hub.docker.com/repository/docker/stubin87/tiny-service)

Will be used with k8s 🚢 and argocd setup 🦑.

Project structure:
 - `argocd` contains config files for setting up argo cd:
    - via helm: `kubectl apply -f argocd/application-via-helm.yaml`
    - via "raw" deployment: `kubectl apply -f argocd/application.yaml`
 - `kubernetes-tiny-api` contains k8s config files for tiny-api deployment (with `ingress`)
 - `kubernetes-tiny-api-with-lb-svc` contains k8s config files for tiny-api deployment (with service of type LoadBalancer)
 - `kubernetes-tiny-web-client` contains k8s config files for tiny-api web client (vue/vuetify web app) deployment (with `ingress`)
 - `kubernetes-tiny-web-client-with-lb-svc` contains k8s config files for tiny-api web client (vue/vuetify web app) deployment (with service of type LoadBalancer)
 - `kubernetes-dashboard` contains some config files for k8s dashboard with necessary permissions for dash to be able to have full access to the cluster
 - `charts` contains packaged helm charts
 - `tiny-api-chart` contains helm chart project for tiny-api
    - to update / create new package:
       - 1 - make changes and update the version in `./tiny-api-chart/Chart.yaml`
       - 2 - `helm package tiny-api-chart`
       - 3 - move the generated file in the `./charts` folder (e.g. `mv tiny-api-chart*.tgz charts`)
       - 4 - `helm repo index . --url https://2beens.github.io/tiny-api-k8s/`
       - 5 - push changes
    - but, creating a helm package is not necessary, argocd can get the app config from this folder (check argocd/application-via-helm.yaml)
 - `index.yaml` required config for the helm repo that is served via github pages

### TODOs (✅ done ✅):
 1. add and setup this repo 
 2. add CI to [tiny-api](https://github.com/2beens/tiny-api) project, to:
    - build and push the docker image with new version/tag on each merge to master
    - edit this repo here, to update the config files with new docker image version
    - watch ArgoCD do it's thing 😍

### TODOs 2 (✅ partially done ⚠️):
 - https://kubernetes.github.io/ingress-nginx/deploy/baremetal/
 - add a github action to create a helm package on push to master:
   - https://helm.sh/docs/howto/chart_releaser_action/
   - NOT NEEDED, because argocd can read helm chart:
        ```yaml
        ...
         source:
          repoURL: https://github.com/2beens/tiny-api-k8s.git
          targetRevision: HEAD
          path: tiny-api-chart
          helm:
            valueFiles:
              - values.yaml
         ...
        ```
         🚨 does not work
 - create a simple frontend project that connects and uses tiny-api and is deployed in the cluster
   - use internal service for communication between web client and API
 - self managed Argo CD:
    - https://medium.com/devopsturkiye/self-managed-argo-cd-app-of-everything-a226eb100cf0

### TODOs 3 (✅ done ✅):
 - MANAGED K8S 😍😍 with DigitalOcean, 200$ free credit for 60 days <3 

#### K8S with ArgoCD setup in DigitalOcean works like a charm (1st of Oct. 22) 👌❤️:

![do-argocd-tiny-api](https://user-images.githubusercontent.com/1224689/193412902-0290d57c-2c9c-4a80-9bd4-07f3feb66db1.jpg)

![do-argocd](https://user-images.githubusercontent.com/1224689/193412960-6cff763d-1723-431e-9fdb-8d1eb579145e.jpg)

### TODOs 4:
 - experiment with different marketplace apps in DigitalOceal k8s setup
 - experiment with different Helm packages (prometheus/grafana?)

### TODOs 5:
Experiment with:

 - internal (within cluster) communication between different services
 - DBs in k8s (stateful set, persistent volumes ...)
 - gRPC (https://grpc.io/docs/languages/go/basics/)

To achieve that, create another tiny service, which communicates with this one via GRPc. This one will send it some data (via GRPc), which the tiny service will store into DB (mongo) (basically CRUD operations on some data). Some data will be simple stock exchange data (stocks and updates of their price).
