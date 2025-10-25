# Build CD
1. go to gh action ci.yaml change it to cicd.yaml
2. change the job name to be ci
3. add a new job called cd.
4. this new job is running on self-hosted and should run 
5. create a secret ARGOCD_PASSWORD
6. argocd login argocd.test.com --insecure --grpc-web --username admin --password ${{ secrets.ARGOCD_PASSWORD }} 
7. argocd app sync python-app
8. THIS DNS is not avaialable on gh runner argocd.test.com 
   1. check
      1. kubectl get pods -n actions-runner-system 
      2. kubectl exec -ti self-hosted-runners-n2nn6-lgpsd -n actions-runner-system -- sh
      3. hostname 
      4. check whether you can access 
      5. curl https://argocd.test.com 
         failed to connect
   2. since they are using the same cluster its easy
      1. kubectl get svc -n argocd -> argocd-server where we want to connect
      2. go to the pod  kubectl exec -ti self-hosted-runners-n2nn6-lgpsd -n actions-runner-system -- sh
      3.  curl https://argocd-server - again the same probl
      4. curl -k https://argocd-server.argocd  add the namespace
      5. this is the endpoint to use
   3. now make the jobs dependend from each other so first ci then cd 
      1. 