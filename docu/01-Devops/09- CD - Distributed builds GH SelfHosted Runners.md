# configure GH runners in cluster

1. Go to google adn search for github runners kubernetes ->  https://github.com/actions/actions-runner-controller
2. Go to Quickstart guide -> https://github.com/actions/actions-runner-controller/blob/master/docs/quickstart.md
3. Go to Gettinh started -> https://github.com/actions/actions-runner-controller/blob/master/docs/quickstart.md#getting-started
   1. Create K8s- we have already
   2. then have a cert manager
      kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.8.2/cert-manager.yaml
   3. kubectl get ns
   4. kubectl get pods -n cert-manager and waitn untill all pods are running
4. Create PAT on GH
   1. REPO selected
   2. only repo
   3. generate token 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
5. Deploy Controller
   1. Choose helm way
      a.ADD REPO helm repo add actions-runner-controller https://actions-runner-controller.github.io/actions-runner-controller
      b. Run it
         helm upgrade --install --namespace actions-runner-system --create-namespace\
         --set=authSecret.create=true\
         --set=authSecret.github_token="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"\
         --wait actions-runner-controller actions-runner-controller/actions-runner-controller
      c. check if correct
         kubectl get pods -n actions-runner-system
   2. create runner 
      1. Run this  Kubectl get pods -n actions-runner-system
         cat << EOF | 
         apiVersion: actions.summerwind.dev/v1alpha1
         kind: RunnerDeployment
         metadata:
         name: self-hosted-runners
         spec:
         replicas: 1
         template:
            spec:
               repository: taulantracaj-gif/python-app
         EOF
      2. kubectl get pods -n actions-runner-system
      3. Go to GH Repo -> settings -> actions -> runners

6. Define Labels for runners
   1. see label self-hosted
   
7. Run using self hosted
   1. Go to the action and un runs-on put the  label self-hosted
   2. it will be running on any runenr which has self-hosted label
8. WHEN Running there is an error  Error: Docker buildx is required. See https://github.com/docker/setup-buildx-action to set up buildx.
      1. keep using ubuntu for building and pushing
      2. for deploy use the self hosted
   