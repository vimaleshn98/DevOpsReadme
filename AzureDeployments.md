# Creating AKS Cluster

#### LINK : 
https://learn.hashicorp.com/tutorials/terraform/aks?in=terraform/kubernetes

#### Through Azure CLI :
https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough

#### Azure CLI Download
https://docs.microsoft.com/en-us/cli/azure/install-azure-cli

```
git clone https://github.com/hashicorp/learn-terraform-provision-aks-cluster
cd learn-terraform-provision-aks-cluster
az ad sp create-for-rbac --skip-assignment (Save the JSON File for Credentials)
```
(Do the last step in cmd if VSCode isnt supporting)

* Run az login and connect your azure account from cmd

* Copy appId and Update in terraform.tfvars file
* Copy Password and Update in terraform.tfvars file
* terraform init
* terraform plan
* terraform apply

* Once cluster is deployed
* Go to azure portal > AKS
* Open the cluster
* First Refresh and then hit Connect button
* Copy the commands that show up on a new window one after another and run it in cmd
* Example : (May Vary in your case) 
```
az account set --subscription 391f406d-5716-467c-b6e5-815b8af91680

az aks get-credentials --resource-group <RG Name> --name <Cluster Name> 

```
#### O/P : Merged ...


* Checking Connection (Local sys interacting with aks cluster)
```
kubectl get pods
```

# Creating Private Container Registry

#### ACR (Azure Container Registry : Its a private registry for private docker Images)

#### LINK :
https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-azure-cli

```
az group create --name myContainerRG --location eastus
az acr create --resource-group myContainerRG --name myContainerRegistrySomeUniqueName --sku Basic
```
#### OUTPUT
```
{
  "adminUserEnabled": false,
  "creationDate": "2019-01-08T22:32:13.175925+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry007",
  "location": "eastus",
  "loginServer": "mycontainerregistry007.azurecr.io",
  "name": "myContainerRegistry007",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

* Open Azure Portal
* Open Container Registry
* Open the Created Container Registry Resource
* Settings > Access Keys
* Enable Admin User
* The password can be used for interaction with this resource later

### Run these two commands in cmd
WhereToFind :
Azure > Container registry > Settings > Access keys
```
kubectl create secret docker-registry <Login server> --docker-username=<Container registry name> --docker-password=<?>
OP: 
secret/mycontainerregistrysomeuniquename.azurecr.io created
```
WhereToFind : Azure > Kube Services > 
```
az aks update --name <Kube service name> --resource-group <Kube RG> --attach-acr <Container registry name>
```


## Continuous Integration

* Import the Repo
https://github.com/Harvey2504/todo-app.git

* Make changes in app > Dockerfile
```
FROM maven:3.5-jdk-8
COPY target/*.war app.war
ENTRYPOINT [ "java","-jar","app.war" ]
```

* Make changes in k8s > api-dep.yml
```

Goto Container registries on azure portal
Open the registry created
On Overview Page
Copy the Login server endpoint
Add : 
image: mycontainerregistrysomeuniquename.azurecr.io/todo-app:tagVersion

```
### Service Connection 
* Project Settings > Service Connection > Create Service Connection > Docker Registry 
* Choose Azure Container Registry
* Choose Azure Container
* Give a Service Connection Token Name

### Create a CI pipeline 
* Pipeline > create a new Pipeline
* Choose Classic Method
* Select Project from azure Repo
* In config step add maven template
* On pipeline tab give the location of maven pom.xml file
* In copy files give contents as : **/*.war
* Add docker task
* Select Container registry Service Connection Token
* Container Repo : Image name mentioned in api-dep.yml 
* Set path for Dockerfile (Like app/Dockerfile)
* Tags : $(Build.BuildId)
Note : (For YAML Way)
```
Add this line :  tags: '$(Version)' 
in the script of docker task

For versioning of tag names in pipeline
Click on variables > new variable
Name : Version
Value : $(Build.BuildId) [This is an inbuilt variable]
```
* Search for command line task
* This is for tagName variable in api-deployments.yml
```
sed -i ''s/tagVersion/$(Version)/g'' k8s/api-deployment.yaml
```
* Add 2 tasks of Publish Artifact : drop

Publish Build Artifacts Task (for database deployment)
Path to Publish : k8s/database-deployment.yaml

Publish Build Artifacts Task (for api deployment)
Path to Publish : k8s/api-deployment.yaml

* Trigger tab : Enable Continuous Integration
* Save and run the pipeline

### Create Service Connection for k8s
* Project Settings > Service Connections > New Service Connection > Kubernetes
* Select Azure Sub (Default)
* Namespace : default
* Service Connection name : kube-connection
* Save


## Continuous Deployment 
* Pipelines > Releases > New Pipeline
* Choose Empty Job Template
* Add Artifacts > Select the CI Build pipeline to access
* Give Specific Version or latest as per requirements
* Enable Continous Deployment Trigger 
* Name the first stage as db-release
* Add another stage named api-release
* Click on 1job,0task (Similar for stage1 $ stage2)
* Add kubectl task
* Choose your kube service connection token from dropdown
* Command : apply
* Select use configuration > file path 
* In file path : select the yaml file to be deployed (stage1 then stage2)
* Save once both stages are configured
* Release


 

### Some Commands To Track Deployments

kubectl get deployment
kubectl delete deployment <deployment name>
kubectl delete deployment --all
kubectl get pods
kubectl get svc (Get the loadbalancers ext. IP for api app)
kubectl describe pod <pod name>

#### Access Link : Loadbalancer Ext Ip:8080 (For This ToDo App)




## Azure Deployments for .net project (Assignment)
#### Documentation
https://azuredevopslabs.com/labs/vstsextend/kubernetes/#before-you-begin
#### Video Description
https://www.youtube.com/watch?v=4DUhc0MjdUc



az aks get-credentials --resource-group <RG Name> --name <Cluster Name> 