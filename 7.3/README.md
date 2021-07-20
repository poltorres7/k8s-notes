### Manejo de recursos  


Crear namespace de trabajo y desplegar big-resources
```
kubectl create ns <NAME>
kubectl apply -f big-resources.yaml -n diegoazd
```  



Revisar fase y status del Pod

```
kubectl get po my-pod-req -n <NAME> -o yaml | grep phase
kubectl get po my-pod-req -n <NAME> -o json | jq .status.containerStatuses
kubectl describe po kubia | grep Containers: -A15
kubectl get po kubia -o json | jq .status.containerStatuses
```
