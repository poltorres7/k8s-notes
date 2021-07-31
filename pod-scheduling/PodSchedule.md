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
```
