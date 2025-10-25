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
      1. go to google and write job dependency github actions -> https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-jobs
      2. use need -> https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-jobs#example-requiring-successful-dependent-jobs
   4. add a task to install argocd

   # Pick the version
   1. capture the tage
   2. modify values and push
   3. look for job outputs -> https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/pass-job-outputs
   4. commit_id
   5. install https://pypi.org/project/yq/
   6. go to terminal 
   7.  kubectl exec -ti self-hosted-runners-n2nn6-knfbd -n actions-runner-system -- sh 
      1.check if it has python and pip
   8. pip install yq
   9. change values.yaml tag to 
      yq -Yi '.image.tag = "${{needs.ci.outputs.commit_id}}"' charts/python-app/values.yaml
   10. push to gh
      1. go to google and search github push github actions -> https://stackoverflow.com/questions/57921401/push-to-origin-from-github-action
      2. clone repo - uses: actions/checkout@v3
      3.   - name: Commit changes
        uses: EndBug/add-and-commit@v9
        with:
          author_name: Your Name
          author_email: mail@example.com
          message: 'Your commit message'
          add: 'report.txt'
       
   11. Permission setup
      1. Go to settings of repo 
      2. Actions - > General
      3. under workflow permissions -> Read and write permissions
    