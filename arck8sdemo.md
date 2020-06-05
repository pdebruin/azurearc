# Azure Arc enabled Kubernetes demo

In this walkthrough you will first register your Kubernetes cluster with Azure Arc. Afgter that you can set a  configuration that will be pulled into your cluster.

## Registering your Kubernetes cluster to Azure Arc

verify you have helm installed
verify kubectl's context is set to the cluster you want to connect


```
az login

az provider register --namespace Microsoft.Kubernetes
az provider show -n Microsoft.Kubernetes -o table

az provider register --namespace Microsoft.KubernetesConfiguration
az provider show -n Microsoft.KubernetesConfiguration -o table

az extension add --name connectedk8s
az extension add --name k8sconfiguration

az extension update --name connectedk8s
az extension update --name k8sconfiguration

resourcegroup=arcrg
name=ddk8sarc
az group create --name $resourcegroup -l westeurope
az connectedk8s connect --name $name --resource-group $resourcegroup
az connectedk8s list -g $resourcegroup -o table

```


## Setting GitOps configuration

text

```
az k8sconfiguration create \
    --name cluster-config \
    --cluster-name $name --resource-group $resourcegroup \
    --operator-instance-name cluster-config --operator-namespace cluster-config \
    --repository-url https://github.com/pdebruin/cluster-config \
    --scope cluster --cluster-type connectedClusters
```
https://github.com/pdebruin/cluster-config
https://github.com/Azure/arc-k8s-demo
