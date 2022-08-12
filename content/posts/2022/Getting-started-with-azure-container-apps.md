
---
title: "Getting started with Azure Container Apps"
date: 2022-08-12T12:59:27+08:00
lastmod: 2022-08-12T12:59:27+08:00
draft: false
categories: ["Azure", ]
tags: ["Azure", "Container Apps", ]
---

Azure Container Apps went [General Available](https://techcommunity.microsoft.com/t5/apps-on-azure-blog/azure-container-apps-general-availability/ba-p/3416885) in May of 2022 and offers a great way to build cloud native apps with serverless containers.

Azure Container Apps behind the scenes is based on Kubernetes, it has been set up and is maintained by Microsoft. The service is setup in a `serverless` way where you only have to worry about deploying your apps and not have to worry about maintanance.

### Features
A couple of features that can be used are:

- Run Multiple container revisions
- Autoscale
- HTTPS ingress
- Internal ingress and service discovery
- Run containers from any registry
- DAPR support

For a complete list of features you can visit the [docs](https://docs.microsoft.com/en-us/azure/container-apps/overview).

In the getting started example the following features are used:
- HTTPS ingress
- Internal ingress and service discovery
- Run containers from any registry

### Getting started case
- Deployment of an webapplication that is public available.
- Deployment of an API that is only available inside the Container Apps Network.

![](images/ACE-diagram.png)

Steps to perform:

1. Create Azure Container Registry (ACR) resource
2. Upload API and APP image to ACR (Source code available on [Github](https://github.com/arnoldboersma/azure-container-apps-demo))
3. Create Azure Container Apps Environment
4. Create APP and API Container App


### Prerequitites
- An Azure account with an active subscription.
- [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)

#### Setup
sign in to Azure from the CLI, and set your default subscription if you have access to multiple subscriptions.
```Bash
az login
az account set --subscription <<subscriptionid>>
```

Install the Azure Container Apps extension for the CLI.
```Bash
az extension add --name containerapp --upgrade
```

Register the `Microsoft.App` namespace, and the `Microsoft.OperationalInsights` provider for the [Azure Monitor Log Analytics Workspace](https://docs.microsoft.com/en-us/azure/container-apps/observability?tabs=bash#azure-monitor-log-analytics) if you have not used it before.
```Bash
az provider register --namespace Microsoft.App
az provider register --namespace Microsoft.OperationalInsights
az provider register --namespace Microsoft.ContainerRegistry
```

#### Setup Azure Resource group
Set a couple of variables
```powershell
$RESOURCE_GROUP="rg-mycontainer-apps"
$LOCATION="westeurope"
$CONTAINERAPPS_ENVIRONMENT="my-environment"
$ACR_NAME="acrmycontainerapps01"
```
Create the resource group
```powershell
az group create `
  --name $RESOURCE_GROUP `
  --location $LOCATION
```


#### Create an Azure Container Registry
Store custom containers in ACR to use with Azure Container Apps.

```powershell
az acr create `
  --resource-group $RESOURCE_GROUP `
  --name $ACR_NAME `
  --sku Basic `
  --query loginServer `
  --output tsv
```

##### Upload images to ACR
Build the images and upload to ACR

```Powershell
$ACR_SERVER=$(az acr show -n $ACR_NAME --query loginServer -o tsv)
az acr build -r $ACR_NAME -t $ACR_SERVER/app:v1 ./azure-container-apps-demo -f ./azure-container-apps-demo/app/Dockerfile
az acr build -r $ACR_NAME -t $ACR_SERVER/api:v1 ./azure-container-apps-demo -f ./azure-container-apps-demo/api/Dockerfile
```
![](images/ACR.png)
#### Create an Azure Container Apps environment

```powershell
az containerapp env create `
  --name $CONTAINERAPPS_ENVIRONMENT `
  --resource-group $RESOURCE_GROUP `
  --location $LOCATION
```

![](images/ContainerAppEnvironment.png)

#### Create containers

Create the containers in Azure Container Apps and configure with the settings:
- API
  - Ingress: Internal -> API is only available internally
- APP
  - Ingress: External -> APP is public available
  - env-vars: Set the api base url in the environment settings which is used in the app to determine the url of the API

```powershell
az acr update --name $ACR_NAME --admin-enabled 
$password=$(az acr credential show --name $ACR_NAME --query 'passwords[0].value' -o tsv)

az containerapp create `
  --name api1 `
  --resource-group $RESOURCE_GROUP `
  --environment $CONTAINERAPPS_ENVIRONMENT `
  --image $ACR_SERVER/api:v1 `
  --ingress 'internal' `
  --target-port 80 `
  --registry-server $ACR_SERVER `
  --registry-username $ACR_NAME `
  --registry-password $password `
  --query properties.configuration.ingress.fqdn
                       
$api_base_url = $(az containerapp show --name api --resource-group $RESOURCE_GROUP --query 'properties.configuration.ingress.fqdn' -o tsv)

az containerapp create `
  --name app1 `
  --resource-group $RESOURCE_GROUP `
  --environment $CONTAINERAPPS_ENVIRONMENT `
  --image $ACR_SERVER/app:v1 `
  --target-port 80 `
  --ingress 'external' `
  --registry-server $ACR_SERVER `
  --registry-username $ACR_NAME `
  --registry-password $password `
  --env-vars API_BASE_URL=$api_base_url `
  --query properties.configuration.ingress.fqdn

```
After creating all resources you should have the following resources in azure:

![](images/caallresources.png)

You can test if the app is working by openening the app, the url of the app can be found in the settings of the container app:

![](images/containerappsettings.png)

The first request to the site could be slow as the [default](https://docs.microsoft.com/en-us/azure/container-apps/scale-app) settings of this app running on 0 instances.

![](images/containerappsresult.png)

### Resources
- [Quickstart: Deploy your first container app](https://docs.microsoft.com/en-us/azure/container-apps/get-started?tabs=bash)
- [Source code on Github](https://github.com/arnoldboersma/azure-container-apps-demo)