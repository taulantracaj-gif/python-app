1. go to https://argocd.test.com/applications
2. Applications
3. New app
4. python-app
5. click auto-create namespace - similar to --create-namesapce in helm
6. Source -> https://github.com/taulantracaj-gif/python-app.git
7. choose the branch
8. go to repo and find the files -> charts/python-app
9. destination -> https://kubernetes.default.svc
10. namespace python
11. helm
    1. values file -> value.yaml
    2. check if the values are loadd
12. Create
13. click on the app
    1. click on SYNC -> SYNCHRONIZE
    2. APP IS DEPLOYED
14. GO TO BROWSER -> http://python-app.test.com/api/v1/info
15. GO TO K8S
   1. kubectl get ns
   2. kubectl get pods -n python
   3.  kubectl get svc -n python
   4.  kubectl get ing -n python