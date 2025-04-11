Prerequisites:
1. Install Docker: https://docs.docker.com/engine/install/
2. Install Kind: https://kind.sigs.k8s.io/docs/user/quick-start/
3. WSL or Powershell
4. Helm
5. Kubectl

Steps:
1. Make Sure Docker is Running
2. Using kind do the following:
    - kind create cluster --name synkronous --config kind-config/kind-config.yaml
3. Install ingress nginx:
    - kubectl apply -f https://kind.sigs.k8s.io/examples/ingress/deploy-ingress-nginx.yaml
4. Create namespaces for argocd, dev and qa
    - kubectl create ns dev
    - kubectl create ns qa
    - kubectl create ns argocd

5. Install ArgoCD
    - helm install argocd argo/argo-cd -n argocd

6. Apply argocd apps to namespace
    - dev: kubectl apply -n argocd -f argo/applications/dev-frontend.yaml
    - qa: kubectl apply -n argocd -f argo/applications/qa-frontend.yaml

7. Wait to sync or check via locahost ui of ArgoCD
    -  kubectl port-forward svc/argocd-server -n argocd 8080:443
    -  get pass: kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 -d
    - login via username admin

8. Test via curl
    - curl -H "Host: dev-front-end.local" http://localhost:8080
    - curl -H "Host: qa-front-end.local" http://localhost:8080