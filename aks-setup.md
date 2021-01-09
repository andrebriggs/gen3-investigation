# Quick and dirty AKS setup

1. Create AKS instance

```console
$ az group create -n abrig-gen3 -l westus
```

```
$ az aks create --resource-group abrig-gen3 --name abrig-gen3-dev --node-count 3 --enable-addons monitoring --generate-ssh-keys --output yaml
```

```
az aks get-credentials --resource-group abrig-gen3 --name abrig-gen3-dev
```

2. Follow Instructions [here](https://github.com/uc-cdis/cloud-automation/blob/master/doc/gen3OnK8s.md)

Create 

`export GEN3_HOME=/Users/andrebriggs/Code/gen3-cloud-automation` 



Finally

Delete cluster

`az group delete --name abrig-gen3 --yes --no-wait`

Randy Olinger has `fence` working iwth Az Storage in a fork
* https://github.com/rolinge/fence

https://github.com/rolinge/cdis-data-client
https://github.com/uc-cdis/compose-services

## Paid alternatives to Gen3
* [Data Biology](https://www.databiology.com/)
* [Seven Bridges](https://www.sevenbridges.com/)


```console
az storage account create -n abriggen3storage -g abrig-gen3 -l westus --sku Standard_LRS
```

```console
az storage queue create --name $queuename --account-name $storagename
```

```console
# Enable Event grid
az provider register --namespace Microsoft.EventGrid
```

For `ssjdispatcher` version for Azure blobs we have a few approaches
* Setup Azure storage account
* Setu