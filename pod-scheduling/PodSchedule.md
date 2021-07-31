### Pod Scheduling  

#### Node Selector  

```
# Ver los nodos por capacityType
kubectl get nodes --label-columns=eks.amazonaws.com/capacityType   
# Abrir el archivo node-selector.yaml
# Ver el archivo y reemplazar <NAME>
# Desplegar el archivo
kubectl apply -f node-selector.yaml
# Se asigno a algun NODO?
```
