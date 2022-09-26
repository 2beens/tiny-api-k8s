# Tiny API with Kubernetes experimentations
k8s config files for the [tiny-api](https://github.com/2beens/tiny-api) project.

Will be used with k8s 🚢 and argocd setup 🦑.

Project structure:
 - `argocd` contains config files for setting up argo cd:
    - via helm: `kubectl apply -f argocd/application-via-helm.yaml`
    - via "raw" deployment: `kubectl apply -f argocd/application.yaml`
 - `kubernetes` contains k8s config files for tiny-api deployment
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

### TODOs:
 1. add and setup this repo 
 2. add CI to [tiny-api](https://github.com/2beens/tiny-api) project, to:
    - build and push the docker image with new version/tag on each merge to master
    - edit this repo here, to update the config files with new docker image version
    - watch ArgoCD do it's thing 😍

### TODOs 2:
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
 - create a simple frontend project that connects and uses tiny-api and is deployed in the cluster
   - use internal service for communication between web client and API 
