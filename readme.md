This project outlines the steps to setup a multi-node cluster using Kubernetes in Docker (KIND)

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
6. Deploy Kafka-Bonzai
