### Pod Scheduling  

#### Node Selector  

```
# Ver los nodos por capacityType
kubectl get nodes --label-columns=eks.amazonaws.com/capacityType   
# Abrir el archivo node-selector.yaml
# Desplegar el archivo
kubectl apply -f node-selector.yaml -n <NAME>

# ¿Se asigno a algun NODO?

kubectl describe pod nginx -n <NAME>
kubectl get node
kubectl label node NODE_NAME curso=CEROUNO
```  

#### Node Name  

```
#Ver los nodos
kubectl get nodes
#Editar el archivo node-name con cualquier node name y reemplazar <NAME>
kubectl apply -f node-name.yaml -n <NAME>
kubectl get pod nginx -o jsonpath='{.spec.nodeName}' -n <NAME>
```  

#### Node Affinity  

```
kubectl apply -f node-affinity.yaml -n <NAME>

#¿Que nodo se asigno?
kubectl get nodes $( kubectl get pod nginx -o jsonpath='{.spec.nodeName}' -n <NAME>) --show-labels

#Eliminar PODS, eliminar label y agregar la etiqueta curso:CEROUNO a un node que este en la zona us-east-1b
kubectl get nodes -l topology.kubernetes.io/zone=us-east-1b
kubectl label node NODE_NAME curso=CEROUNO
kubectl get nodes $( kubectl get pod nginx -o jsonpath='{.spec.nodeName}' -n <NAME>) --show-labels
kubectl label node NODE_NAME curso-
```  

#### Inter podAffinity/podAntiAffinity  

```
# Revisar el archivo pod-affinity.yaml ¿Que hace el deployment?
# Reemplazar la etiqueta <NAME>
kubectl apply -f pod-affinity.yaml -n <NAME>

# ¿Se asignaron los pods a de redis y nginx al mismo nodo?
kubectl get pod <REDIS_PODNAME> -o jsonpath='{.spec.nodeName}' -n <NAME>
kubectl get pod <WEBSERVER_PODNAME>  -o jsonpath='{.spec.nodeName}' -n <NAME>

kubectl describe node <NODE_NAME>
```  

####  Taint y Tolerations  

```
kubectl taint nodes <NODE_NAME> env=staging:NoSchedule  
kubectl taint nodes <NODE_NAME> team=qa:NoSchedule  
# Abrir el archivo tolerations.yaml, reemplazar <NAME>
kubectl apply -f tolerations. -n <NAME>
kubectl describe node $(kubectl get pod nginx  -o jsonpath='{.spec.nodeName}' -n <NAME>)

#¿Se asigno al nodo con los taints?
# Agregar el toleration team=qa
```  
