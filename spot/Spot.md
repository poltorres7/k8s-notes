### Optimizacion de costos  
Con estos comandos veremos como asignar una carga de trabajo a insntacias de tipo SPOT y como identificar   
```
#Con este comando se veran las diferentes instancias por capacidad computacional
kubectl get nodes --label-columns=eks.amazonaws.com/capacityType

#Filtramos las instancias para que se muestren solo las tipo SPOT
kubectl get nodes --label-columns=eks.amazonaws.com/capacityType --selector=eks.amazonaws.com/capacityType=SPOT
```  

Editar el archivo nginx-spot.yaml y reeamplazar <NAME>  
```
kubectl apply -f nginx-spot.yaml  

#Muestra el nodo asignado al pod
kubectl get pod nginx-spot-<NAME> -o jsonpath='{.spec.nodeName}'

#Muestra informacion del pod al que se asigno
kubectl describe node $(kubectl get pod nginx-spot-<NAME> -o jsonpath='{.spec.nodeName}')
```
