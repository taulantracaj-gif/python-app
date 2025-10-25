1. go to google and search argocd cli install -> https://argo-cd.readthedocs.io/en/stable/cli_installation/
2. use linux and go with curl Linux and WSL
   curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
    mv argocd-linux-amd64 ~
    chmod +x argocd-linux-amd64
    sudo mv argocd-linux-amd64 /usr/local/bin/argocd
    argocd
    argocd version
