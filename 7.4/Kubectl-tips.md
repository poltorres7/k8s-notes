### kubectl tips  
- Creacion de objetos  
- Buscando recursos  
- Actualizacion de recursos  
- Buscando recursos  
- Borrando recursos  
- Interactuando con recursos  
- Obteniendo ayuda  

#### Creacion de objetos  

```
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

sed -i 's/<NAME>/diegoazd/g' nginx.yaml | kubectl apply -f -
```
