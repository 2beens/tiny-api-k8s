# tiny-api-k8s
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
 - `index.yaml` required config for the helm repo that is served via github pages

### TODOs:
 1. add and setup this repo 
 2. add CI to [tiny-api](https://github.com/2beens/tiny-api) project, to:
    - build and push the docker image with new version/tag on each merge to master
    - edit this repo here, to update the config files with new docker image version
    - watch ArgoCD do it's thing 😍

### TODOs 2:
 - add a github action to create a helm package on push to master:
   - https://helm.sh/docs/howto/chart_releaser_action/
