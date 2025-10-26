Before moving on, make sure to delete all the kubernetes objects that you created from <your-python-app>/k8s.

You can do so as follows:

cd ~/python-app/k8s
 
# Delete the ingress
kubectl delete -f ingress.yaml
 
# Delete the service
kubectl delete -f service.yaml
 
# Delete the deployment
kubectl delete -f deploy.yaml

# Install Helm
go to browser and then helm install
https://helm.sh/docs/intro/install/ 

1. Linux
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
mv get_helm.sh ~
$ chmod 700 get_helm.sh
$ chmod +x get_helm.sh
$ ./get_helm.sh
$ helm version

2. Create Helm Chart
create folder charts
go in there
helm create python-app
remove charts and helmignore folder inside python-app 
rm -rf charts/
rm -rf .helmignore
remove tests under template folder, hpa, service account
go to values.yaml
     1.change add change service, ingress, deployment
     2.to check which ingress class to put
     3.kubectl get ingressclass
     4.change the host to python-app.test.com
     5. pathType change it to Prefix
     6. and serviceAccount set it to false
          serviceAccount:
               create: false
     7. change liveness and rediness probe to /api/v1/healthz

3. Dpeploy Helm
     1. GO TO the right directory -> /mnt/c/Projects/Platform Engineer/python-app-aplatform/charts/python-app
     2. Once there run : helm install python-app -n python . (installation name) -> namespace does not create but the helm will create it and we have . which is where is the chart where we are going to use
     3. when you run helm install python-app -n python . you will get namespace python does not exist
     4. confirm kubectl get ns
     5. Run using flag helm install python-app -n python . --create-namespace 
     6. chek again the namespace -> kubectl get ns
     7. check pod -> kubectl get pods -n python
     8. check ingress -> kubectl get ing -n python
     9. check service -> kubectl get svc -n python
     10. check deployment -> kubectl get deployment -n python
     11. to upgrade -> helm upgrade python-app -n python . --install --create-namespace

4. Deelete release before ARGO
     1. helm uninstall python-app -n python