# xport-my-k8s
##How to install kubernetes in AWS using KOPS

**creating S3 bucket for storing cluster configuration**
```
aws s3api create-bucket --bucket imesh-kops-state-store} --region us-east-1
aws s3api create-bucket --bucket imesh-kops-states-store
aws s3api put-bucket-versioning --bucket imesh-kops-states-store --versioning-configuration Status=Enabled
```
**Setting up cluster name configuration in environment variables (Windows)**
```
set KOPS_CLUSTER_NAME=miluchita.k8s.local
set KOPS_STATE_STORE=s3://imesh-kops-states-store
```
**Creating K8s cluster, in this case with only one availability zone**
```
kops create cluster master-count=1 --master-size=t2.micro --node-count=2 --node-size=t2.micro --zones=us-east-1a --name=k8s.miluchita.com.mx 

kops create cluster --master-count=1 --master-size=t2.micro --node-count=2 --node-size=t2.micro --zones=us-east-1a --name=k8s.miluchita.com.mx

kops create cluster --master-count=1 --master-size=t2.micro --node-count=2 --node-size=t2.micro --zones=us-east-1a --name=miluchita.k8s.local
```
** Creating/setting ssh keys for cluster management.
_ _

SSH public key must be specified when running with AWS (create with `kops create secret --name ik8s.miluchita.com.mx sshpublickey admin -i ~/.ssh/id_rsa.pub`)
use ssh-keygen if it does not exist in your system
_ _

```
kops create secret --name miluchita.k8s.local sshpublickey admin -i .ssh/id_rsa.pub

kops update cluster --name miluchita.k8s.local --yes
```
- - once finish you can validate if the cluster was created correctly - -
``` kops validate cluster ```


**create kubernetes dashboard**

the endpoing will be /api/v1/namespaces/kube-system/services/http:kubernetes-dashboard:/proxy/
```
kubectl apply https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta4/aio/deploy/recommended.yaml
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
```
**getting  "admin" password**
```kops get secrets kube --type secret -oplaintext```

**getting cluster information such endpoints**
```kubectl cluster-info```
