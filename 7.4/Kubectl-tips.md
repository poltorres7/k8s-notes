### kubectl tips  
- Creacion de objetos  
- Busqueda de recursos  
- Actualizacion de recursos  
- Buscando recursos  
- Borrando recursos  
- Interactuando con recursos  
- Obteniendo ayuda  

#### Creacion de objetos  

```
kubectl create ns <NAME>

cat << EOF | kubectl apply -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: diegoazd
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
EOF

kubectl delete deployment nginx-deployment -n <NAME>

cat nginx.yaml | sed 's/<NAME>/diegoazd/g' | kubectl apply -f -

kubectl create deployment nginx -n <NAME> --image=nginx:1.14.2 --dry-run -o yaml > name.yaml
```  

#### Busqueda de recursos  
```
# Busqueda de todos los servicios en el namespace
kubectl get services -n <NAME>
```
