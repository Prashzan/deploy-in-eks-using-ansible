# deploy-in-eks-using-ansible

## In this section , we are using terraform to deploy application into a k8 cluster, which is another common use case with ansible that is very widely used with k8 cluster.

### What we will be doing is 
```
1. First we create k8 cluster in aws using terraform script
2. We gonna configure ansible to connect to the eks cluster and deploy a simple deployment and service component inside that cluster
```

### Firt of all we need to update kubeconfig file to be able to connect to cluster. These kubeconfig includes all of the information needed to connect to our cluster like server_address, hostname and certificates and so on.. These information is what we are going to be using to connect to our eks cluster using anisble.

### First we gonna have to generate kubeconfig file so we have the information we need about the cluster before we can connect to it. The command is below

### kubeconfig_myapp-eks-cluster is generated and used on deploy-to-k8s.yaml file

``` aws eks update-kubeconfig --region ca-central-1 --name myapp-eks-cluster --kubeconfig ~/deploy-in-eks-using-ansible/kubeconfig_myapp-eks-cluster ```

### Some required libraries are
```
1. python >= 3.9
2. kubernetes >= 24.20
3. PyYAML >= 3.11
4. jsonpatch
```

``` pip install pyyaml --user ```
### --user makes pip install package in your home directory instead as it requires no special privileges

### Now make kubeconfig available,on terminal, this will connect to eks cluster
``` EXPORT KUBECONFIG = "-----ABSOLUTEPATH ----/kubeconfig_myapp-eks-cluster ```

#### To see all the namespace in cluster
#### you should see my-app namespace in the cluster by running command below
``` kubectl get ns ``` 

#### To see nginx pod is running or not
``` kubectl get pod -n my-app ```

``` kubectl get svc -n my-app ```
#### you should see public load balancer which is assigned a dns name which we can use to access the nginx service


### Set environment variable for kubeconfig
#### It would be very inconvenient to use kuebconfig attribute in every single task. We can define kubeconfig attribute once and basically use for the whole playbook.We can do that using K8S_AUTH_KUBECONFIG environment varibale
``` EXPORT K8S_AUTH_KUBECONFIG = /----absolute-path----/kubeconfig_myapp-eks-cluster ```

### To run the playbook and deploy the nginx app into cluster
``` ansible-playbook deploy-to-k8s.yaml ```


