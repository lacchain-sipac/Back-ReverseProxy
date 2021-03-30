# nginx-proxy

## Configuring the ingress-nignx in azure

* Get the resource group:

```shell
az aks show --resource-group RGprocesosfiduciarios001 --name CLprocesosfiduciarios001 --query nodeResourceGroup -o tsv
```

* Create a static IP:

```shell
az network public-ip create --resource-group MC_RGprocesosfiduciarios001_CLprocesosfiduciarios001_eastus --name CLprocesosfiduciarios001PublicIP001 --sku Standard --allocation-method static --query publicIp.ipAddress -o tsv
#===> 52.147.215.171
```

* Create a namespace

```shell
kubectl create namespace ingress-basic
```

* Add the Official stable repository

```shell
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```

* Adding the ingress-nginx (with a name **nginx-ingress**)

```shell
helm install nginx-ingress stable/nginx-ingress \
    --namespace ingress-basic \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set controller.service.loadBalancerIP="52.147.215.171" \
    --set controller.service.annotations."service\.beta\.kubernetes\.io/azure-dns-label-name"="sipac-invest-honduras"
```

* Verifying the load balancer -nignx configuration:

```shell
kubectl --namespace ingress-basic get services -o wide -w nginx-ingress-controller
kubectl get service -l app=nginx-ingress --namespace ingress-basic
```

* assign the domain to the ip address (assign it by choosing the hostname provider)

## Manual Deployment

```shell
kubectl apply -f k8s/
```
