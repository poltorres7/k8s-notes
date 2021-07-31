### Pod Scheduling  

#### Node Selector  

```
# Ver los nodos por capacityType
kubectl get nodes --label-columns=eks.amazonaws.com/capacityType   
# Abrir el archivo node-selector.yaml
# Ver el archivo y reemplazar <NAME>
# Desplegar el archivo
kubectl apply -f node-selector.yaml

# ¿Se asigno a algun NODO?

kubectl describe pod nginx-<NAME>
kubectl get node
kubectl label node NODE_NAME curso=CEROUNO
```  

#### Node Name  

```
#Ver los nodos
kubectl get nodes
#Editar el archivo node-name con cualquier node name y reemplazar <NAME>
kubectl apply -f node-name.yaml
kubectl get pod nginx-<NAME> -o jsonpath='{.spec.nodeName}'
```  

#### Node Affinity  

```
#Editar el archivo y reemplazar <NAME>
kubectl apply -f node-affinity.yaml

#Eliminar PODS, eliminar label y agregar la etiqueta curso:CEROUNO a un node que este en la zona us-east-1b
kubectl label node NODE_NAME curso-
```  

#### Inter podAffinity/podAntiAffinity  

```
# Revisar el archivo pod-affinity.yaml ¿Que hace el deployment?
# Reemplazar la etiqueta <NAME>
kubectl apply -f pod-affinity.yaml
```
