### Ingress controller  
Instalacion ingress controller  
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm search repo ingress-nginx
helm install ingress-nginx ingress-nginx/ingress-nginx
```  

Editar microservices.yaml y reemplazar la etiqueta <SVC_ADDR> por la direccion del load balancer  
`kubectl apply -f microservices.yaml -n <NAME>`  

Revisar instalacion  
```
kubectl get pods
kubectl get svc
```  

#### Creacion [ingress](https://kubernetes.github.io/ingress-nginx/examples/rewrite/)  
```
#Abrir httpbin.yaml
# reemplazar <SVC_ADDR> con la direccion del service load balancer asociado al ingress controller <NAME> en la seccion path
kubectl apply -f httpbin.yaml -n <NAME>

#Realizar peticion al servicio
curl -I -k http://<SVC_ADDR>/<NAME>/status/200
```  
