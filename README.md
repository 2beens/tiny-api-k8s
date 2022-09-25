# tiny-api-k8s
k8s config files for the tiny-api project

Will be used with k8s 🚢 and argocd setup 🦑.

### TODOs:
 1. add and setup this repo 
 2. add CI to [tiny-api](https://github.com/2beens/tiny-api) project, to:
    - build and push the docker image with new version/tag on each merge to master
    - edit this repo here, to update the config files with new docker image version
    - watch ArgoCD do it's thing 😍

### TODOs 2:
 - add a github action to create a helm package on push to master:
   - https://helm.sh/docs/howto/chart_releaser_action/

## Argo CD setup:

 1 - via helm: `kubectl apply -f argocd/application-via-helm.yaml`
 
 2 - "raw" deployment: `kubectl apply -f argocd/application.yaml`
