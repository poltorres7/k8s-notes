### Optimizacion de costos  
Con estos comandos veremos como asignar una carga de trabajo a instancias de tipo SPOT y como identificar   
```
#Con este comando se verán las diferentes instancias por capacidad computacional
kubectl get nodes --label-columns=eks.amazonaws.com/capacityType

#Filtramos las instancias para que se muestren solo las tipo SPOT
kubectl get nodes --label-columns=eks.amazonaws.com/capacityType --selector=eks.amazonaws.com/capacityType=SPOT
```  

Revisar el archivo nginx-spot.yaml
```
###Crear namespace para trabajar con los cambios
kubectl create ns <NAME>

kubectl apply -f nginx-spot.yaml -n <NAME>

#Muestra el nodo asignado al pod
kubectl get pod nginx-spot -o jsonpath='{.spec.nodeName}' -n <NAME>

#Muestra información del pod al que se asigno
kubectl describe node $(kubectl get pod nginx-spot -o jsonpath='{.spec.nodeName}' -n <NAME>)
```
