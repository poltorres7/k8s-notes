### Sops y kms  
Instalacion sops  
- [osx](https://formulae.brew.sh/formula/sops)  
- rpm
  ```
  wget -O sops.rpm https://github.com/mozilla/sops/releases/download/v3.7.1/sops-3.7.1-1.x86_64.rpm
  sudo yum localinstall sops.rpm
  ```  
- debian
  ```
  wget -O sops.deb https://github.com/mozilla/sops/releases/download/v3.7.1/sops_3.7.1_amd64.deb
  sudo apt install ./sops.deb
  ```  


Configuracion de entorno  
![alt image](images/assumeRole.png)  

#### Configuracion del perfil de AWS  
En ~/.aws/config  
```
[profile cerouno]
region=us-east-1
role_arn=arn:aws:iam::ACCOUNT_ID:role/cerouno-k8s
source_profile=cerouno
```  
En ~/.aws/credentials  
```
[cerouno]
aws_access_key_id = AKI
aws_secret_access_key = ASK
```    

#### Agregar variable de entorno  
```
export=AWS_PROFILE=cerouno
export SOPS_KMS_ARN="arn:aws:kms:us-east-1:ACCOUNT_ID:key/920aff2e-c5f1-4040-943a-047fa387b27e"  
# Verificar que estemos en la cuenta correcta:  
aws sts get-caller-identity --output text --query 'Account'   
```
  
Uso de sops  
```
sops --encrypt secret.yaml > secret.sops.yaml
sops --decrypt secret.sops.yaml | kubectl apply -f -
```  
