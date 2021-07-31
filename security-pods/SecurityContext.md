### Security context  

#### Repaso de usuarios y grupos en linux  
```
docker run -it --rm --tty busybox
#Listar archivo /etc/passwd
ls -l /etc/passwd
-rw-r--r--    1 root     root           340 Jun  6 21:21 /etc/passwd
```  
El primer caracters nos dice que tipo de archivo es:  
* d es un directorio  
* - nos indica un archivo normal  

Los siguientes 3 digitos nos indican los permisos para el dueño del archivo RWX  
Los siguientes 3 digitos nos indican los permisos para los miembros del grupo  RWX  
Los ultimos 3 digitos nos indican los permisos para el resto de los usuarios  

Grupos y usuarios en Linux  
```
cat /etc/passwd
root:x:0:0:root:/root:/bin/sh
daemon:x:1:1:daemon:/usr/sbin:/bin/false
```  
La tercera columna nos indica el id del usuario  
La cuarta columna nos induica el id del grupo primario  

Otra forma de ver los grupos del usuario es con el comando id  
```
/ # id root
uid=0(root) gid=0(root) groups=0(root),10(wheel)
```  
O con el comando groups  
```
/ # groups root
root wheel
```  

#### Security context  
- Ver el archivo sc-demo.yaml  
- Verl el archivo sc-failed-demo.yaml  

Desplegar sc-demo.yaml y conectarse al pod  
```
kubectl apply -f sc-demo.yaml
kubectl exec -it sc-demo /bin/bash
#Dentro del contenedor ver los permisos del archivo y los usuarios del contenedor
ls -la /etc/message
cat /etc/passwd
```  

Desplegar sc-failed-demo.yaml  
```
kubectl apply -f sc-failed-demo.yaml
kubectl logs sc-failed-demo
```
