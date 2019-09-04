# Instructions
- make cluster `infra-master`
- Run terraform to create subnets (one VPC / org? One routing VPC) Add bastion host.
- Create kustomize overlay
- `kustomize build overlays/<my cluster> | eksctl create cluster -f -` (`kustomize build cluster/overlays/infra-master |eksctl --profile sap-eks create cluster -f -`)
- bootstrap cluster:
    - gvisor
    - PodNodeSelector, missing in AWS EKS 
    - psp
    - argo cd
        `kubectl create namespace argocd`
        `kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml`
    - alb ingress
    - setup storageclasses
    - cilium
    - auth
    - istio
    - ingress (https://istio.io/blog/2019/custom-ingress-gateway/)
    - kubernetes dashboard (kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta1/aio/deploy/recommended.yaml)

http://bluechiptek.com/blog/understanding-role-based-access-control-rbac-with-amazon-eks-part-1

Cluster layout:
    infra-cluster
        xpr-production
        xpr-test
        bn-production
        bn-test
        lu-production
        lu-test
        infra-test


## Cilium

`kubectl -n kube-system set env ds aws-node AWS_VPC_K8S_CNI_EXTERNALSNAT=true`
`kubectl -n kube-system apply -f https://raw.githubusercontent.com/cilium/cilium/1.5.5/examples/kubernetes/node-init/eks-node-init.yaml`
`kubectl apply -f https://raw.githubusercontent.com/cilium/cilium/1.5.5/examples/kubernetes/1.13/cilium.yaml` 

