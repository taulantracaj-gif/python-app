# INSTALL ARGO CLI
1. go to https://argocd.test.com/
2. login 
3. create the repo if not existance
4. go applications NEW APP
  1. give a name
5. sync
6. GO TO K8S
   1. kubectl get ns
   2. kubectl get pods -n python
   3.  kubectl get svc -n python
   4.  kubectl get ing -n python
7. go to google argocd cli deploy app ->  https://argo-cd.readthedocs.io/en/stable/getting_started/
8. Login to argo with no certificate
   argocd login argocd.test.com --insecure --grpc-web --username admin --password XdkccEaVwXxMhO9A

# SYNC ARGO CD APPS PROGRAMICALLY
   0.  argocd app -h
   1. argocd app list
   2. argocd app sync -h
   3. argocd app sync python-app