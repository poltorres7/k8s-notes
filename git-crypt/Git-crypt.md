### Git crypt  
- [Instalacion](https://github.com/AGWA/git-crypt/blob/master/INSTALL.md)  
- apt
```
sudo apt-get update -y
sudo apt-get install -y git-crypt
git-crypt version
```  
- rpm    
```
sudo wget https://cbs.centos.org/kojifiles/packages/git-crypt/0.5.0/1.el7/x86_64/git-crypt-0.5.0-1.el7.x86_64.rpm
sudo rpm -ivh git-crypt-0.5.0–1.el7.x86_64.rpm
git-crypt version
```  

### Uso  
Ir a la de kubernetes_avanzado
```
git-crypt init
git-crypt status
```

Dentro del repositorio hay un archivo llamado .gitattributes abrirlo  
El arhivo nos indica que archivos/carpetas deben ser encriptados    
```
secretfile filter=git-crypt diff=git-crypt
*.key filter=git-crypt diff=git-crypt
secretdir/** filter=git-crypt diff=git-crypt
```  
Ir a git-crypt y visualizar el archivo secret.yaml, ¿Se puede ver?  

Para desbloquear el archivo hay que agregar la llave con el siguiente comando    
```
git-crypt unlock KEY_FILE  
# Visualizar el archivo secret.yaml
```  

Para obtener una llave se puede hacer con la siguiente comando
`git-crypt export-key OUTPUT_KEY_FILE_NAME`  

### Agregar un archivo y tambien a .gitattributes
