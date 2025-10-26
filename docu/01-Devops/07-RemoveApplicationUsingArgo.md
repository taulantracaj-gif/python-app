1. go to https://argocd.test.com/applications
2. Applications
3. select your app
4. and click X delete and pass python-app and choose Foreground
5. wait to finish
7. click refresh app select python app
8. go to http://python-app.test.com/api/v1/info and you should get 404
9. GO TO K8S
   1. kubectl get ns
   2. kubectl get pods -n python
   3.  kubectl get svc -n python
   4.  kubectl get ing -n python
10. 