### Security context  
Revisar el archivo [rbac.yaml](https://raw.githubusercontent.com/diegoazd/kubernetes-avanzado/main/security-pods/rbac.yaml)  

Desplegar el archvo rbac.yaml  
```
kubectl apply -f rbac.yaml -n <NAME>
kubect api resources -o wide
kubectl explain deployments  
```  
Referencia del [api de kubernetes](https://kubernetes.io/docs/reference/using-api/#api-versioning)  
- Core: usa el path /api/v1, el api group es usualmente "" o v1  
- Named groups: El resto de las apis usan /apis/GROUP_NAME/$version  

Entrar al pod  
```
kubectl exec -it viewer-pod -n <NAME> /bin/sh
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
¿Que necesitamos para que se pueden ver los deployments para el todos los namespaces?  
