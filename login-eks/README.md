### Configuracion aws  

Login cluster k8s:  
Con eks ya no hay grupos nativos de usuarios de IAM y el cluster de k8s, la autenticación en eks  
se hace por medio de usuarios asociados a roles de IAM.  

![alt login eks](images/RBAC-IAM.png)  

El primer paso para autenticar en eks es configurar nuestro espacio local, hay dos archivos que configurar  
- Agregar en ~/.aws/config  

```
mkdir -p ~/.aws

cat << EoF >> ~/.aws/config

[profile cerouno]
role_arn=arn:aws:iam::${ACCOUNT_ID}:role/cerouno-k8s
source_profile=cerouno

EoF

```  
- Agregar en ~/.aws/credentials  

 ```
 AKI=""
 ASK=""

 cat << EoF >> ~/.aws/credentials

 [cerouno]
 aws_access_key_id=$AKI
 aws_secret_access_key=$ASK

 EoF
 ```   

 Probar si funciona la configuracion:  
 `aws sts get-caller-identity --profile cerouno`  

Login k8s:  
```
aws eks --region us-east-1 update-kubeconfig --name cerouno --profile cerouno
kubectl get pods
```  
