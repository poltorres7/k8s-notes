### Security context  
Revisar el archivo [rbac.yaml](https://raw.githubusercontent.com/diegoazd/kubernetes-avanzado/main/security-pods/rbac.yaml)  

Desplegar el archvo rbac.yaml  
`kubectl apply -f rbac.yaml`  

Entrar al pod
```
kubectl exec -it viewer-pod -n sa-rbac /bin/sh
#Ir a /root/scripts
cd /root/scripts
ls
#Ver los archivos
cat deployment.sh
cat pod.sh
#Ejecutar los archivos
sh deployment.sh
sh pod.sh
```
