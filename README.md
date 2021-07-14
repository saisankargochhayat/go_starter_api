# Go_starter_api

To create cluster - 
```
az aks create -g MyResourceGroup -n MyAKS --location eastus  --attach-acr MyHelmACR --generate-ssh-keys
```


Helm Install API into cluster - 
```
helm create starter-project
```
Do the manual changes to starter-project/values.yaml and templates/deployment.yaml to set the correct port and the right image location. 

```
helm install --namespace starter-project starter-project starter-project
```

To setup reverse proxy - 
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

helm upgrade --install nginx-ingress ingress-nginx/ingress-nginx \
    --namespace starter-project \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set controller.admissionWebhooks.patch.nodeSelector."beta\.kubernetes\.io/os"=linux
```

After finishing the deployment of nginx to link the nginx proxy to our app - 
```
kubectl apply -f templates/starter-project-ingress.yaml 
```

To shut down cluster when not used. 
```
az aks stop --name myAKSCluster --resource-group myResourceGroup
```