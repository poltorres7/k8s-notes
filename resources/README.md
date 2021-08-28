### Manejo de recursos  
Utilerias:  
- [jq](https://stedolan.github.io/jq/download/)  

Crear namespace de trabajo y desplegar big-resources
```
kubectl create ns <NAME>
kubectl apply -f big-resources.yaml -n <NAME>
```  

Revisar fase y status del Pod  

```
Estatus del pods:

kubectl get po big-pod-req -n <NAME> -o yaml | grep phase
kubectl get po big-pod-req -o json -n <NAME> | jq .status
kubectl get po big-pod-req -o yaml -n <NAME> | grep status -A15
kubectl get po big-pod-req -n <NAME> -o json | jq .status
¿Por que no se tiene status de los contenedores?

kubectl delete pod big-pod-req -n <NAME>

Estatus de los contenedores:
kubectl apply -f normal-resources.yaml -n <NAME>
kubectl describe po normal-pod-req -n <NAME> | grep Containers: -A12
kubectl get po normal-pod-req -n <NAME> -o json | jq .status.containerStatuses

Mostar recursos de los contenedores:
 kubectl get po normal-pod-req -n <NAME> -o json | jq .spec.containers[].resources
 kubectl get pod -n <NAME> -o yaml | grep spec: -A15

 kubectl get pod -n <NAME> -o yaml | grep nodeName
 kubectl get pod -n <NAME> -o json | grep nodeName
 kubectl get pod -n <NAME> -o wide

kubectl describe node <NODE_NAME>
kubectl delete pod normal-pod-req -n <NAME>
```  

Probes:  
Startup probe  
```
kubectl apply -f startupProbe.yaml -n <NAME>
watch kubectl get pod -n <NAME>
kubectl delete po startup-exec-pod -n <NAME>
```  

  liveness Probe  
  ```
  kubectl apply -f livenessProbe.yaml -n <NAME>
  watch kubectl get pods -n <NAME>
  kubectl describe pod liveness-pod -n <NAME>  | grep Events: -A 15
  ```  

  Readiness Probe  
  ```
  kubectl apply -f readinessProbe.yaml -n <NAME>
  watch kubectl get pods -n <NAME>  
  kubectl describe pod readiness-pod -n <NAME>  | grep Events: -A 15
  ¿Por que no cambia el estatus?
  ```  
