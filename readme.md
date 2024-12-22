This project outlines the steps to setup a multi-node cluster using Kubernetes in Docker (KIND) and deploy a voting app using argocd declarative objects.
Instead of using a public repo, I have tried setting this project as a private repo to miic real world environments

1. Install kind
   -  choco install kind
2. Install kubectl
   -  choco install kubernetes-cli
3. Define your cluster config.
   -  kind-cluster-1.yaml
4. Create kind cluster
   - kind create cluster --config=kind-cluster-1.yaml
   - check if the cluster works: A 'docker ps' should show three containers running the k8s images'
   - You can also check the cluster config by running 'kind get cluster'
5. Deploy Argocd
   - kubectl create namespace argocd
   - kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   - kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
   - kubectl port-forward -n argocd service/argocd-server 8443:443 &
6. Access Argocd UI
   - get password using - kubectl get secret -n argocd argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo
   - access localhost:8443
   - login with username 'admin' and password from earlier
7. Generate a token on github and create a secret for argocd
8. Create a yaml file for argocd application: argocd-app-voting.yaml
9. Deploy the argocd CRD
   - kubectl apply -f argocd-app-voting.yaml
   
Voila!!!! QED

Clean up: 
   - kind delete clusters <cluster-name>
