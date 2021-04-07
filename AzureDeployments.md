# Creating AKS Cluster

#### LINK : 
https://learn.hashicorp.com/tutorials/terraform/aks?in=terraform/kubernetes

#### Through Azure CLI :
https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough

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

az aks get-credentials --resource-group firm-marlin-rg --name firm-marlin-aks
O/P : Merged ...
```
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



## CI

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

* Project Settings > Service Connection > Create Service Connection > Docker Registry 
* Choose Azure Container Registry
* Choose Azure Container
* Give a Service Connection Token Name

* Create a CI pipeline 
Pipeline > create a new Pipeline
Select Project from azure Repo
In config step add maven template
Edit location of pom file  (Like 'app/pom.xml')
Show Assistant
Add docker task at after maven task scripts
Select Container registry Service Connection Token
Container Repo : Image name mentioned in api-dep.yml 
Set path for Dockerfile (Like app/Dockerfile)
Then succeed with Add Button
Add this line :  tags: '$(version)'

* For versioning of tag names in pipeline
Click on variables > new variable
Name : give variable name
Value : $(Build.BuildId) [This is an inbuilt variable]

* For tagName in deployments
Search for command line task
Put : sed -i ''s/tagVersion/$(version)/g'' k8s/api-deployment.yaml
Add task

* For Deployment

Publish Build Artifacts Task (for database deployment)
Path to Publish : k8s/database-deployment.yaml

Publish Build Artifacts Task (for api deployment)
Path to Publish : k8s/api-deployment.yaml

Save and run the pipeline



 






kubectl get svc