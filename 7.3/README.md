### Manejo de recursos  
Utilerias:  
- [jq](https://stedolan.github.io/jq/download/)  

Crear namespace de trabajo y desplegar big-resources
```
kubectl create ns <NAME>
kubectl apply -f big-resources.yaml -n diegoazd
```  

Revisar fase y status del Pod  

```
Estatus del pods:

kubectl get po big-pod-req -n <NAME> -o yaml | grep phase
kubectl get po big-pod-req -o json -n <NAME> | jq .status
kubectl get po big-pod-req -o yaml -n <NAME> | grep status -A15
kubectl get po big-pod-req -n <NAME> -o json | jq .status
¿Por que no se tiene status de los contenedores?

Estatus de los contenedores:
kubectl describe po my-pod-req -n <NAME> | grep Containers: -A15
kubectl get po my-pod-req -n <NAME> -o json | jq .status.containerStatuses


kubectl delete pod my-pod-req -n <NAME>
```
