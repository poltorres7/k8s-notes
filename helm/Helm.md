
### Helm  
Agregar repositorio stable  
```
helm version
helm repo add stable https://charts.helm.sh/stable
helm repo update
helm search repo wordpress
```   

Chart repository  
```
mkdir test-charts
cd test charts/
helm repo list
helm fetch stable/wordpress
helm fetch stable/spark
helm repo index ./
cat index.yaml

#El resultado es algo similar, notar que mariadb tiene una dependencia con mariadb
apiVersion: v1
entries:
  spark:
  - apiVersion: v1
    appVersion: 1.5.1
    created: "2021-08-10T18:24:15.570775-05:00"
    deprecated: true
    description: DEPRECATED - Fast and general-purpose cluster computing system.
    digest: e0b4862812f85dcf7b92d71ed90e9f894f1aef35bb76f384fe322ba736cd10fc
    home: http://spark.apache.org
    icon: http://spark.apache.org/images/spark-logo-trademark.png
    name: spark
    sources:
    - https://github.com/kubernetes/kubernetes/tree/master/examples/spark
    - https://github.com/apache/spark
    urls:
    - spark-1.0.5.tgz
    version: 1.0.5
  wordpress:
  - apiVersion: v1
    appVersion: 5.3.2
    created: "2021-08-10T18:24:15.576387-05:00"
    dependencies:
    - condition: mariadb.enabled
      name: mariadb
      repository: https://kubernetes-charts.storage.googleapis.com/
      tags:
      - wordpress-database
      version: 7.x.x
    deprecated: true
    description: DEPRECATED Web publishing platform for building blogs and websites.
    digest: b83c4d4009d3a37298a13e75afa1a16f1826b3f84504d1201c1bd7e301501f44
    home: http://www.wordpress.com/
    icon: https://bitnami.com/assets/stacks/wordpress/img/wordpress-stack-220x234.png
    keywords:
    - wordpress
    - cms
    - blog
    - http
    - web
    - application
    - php
    name: wordpress
    sources:
    - https://github.com/bitnami/bitnami-docker-wordpress
    urls:
    - wordpress-9.0.3.tgz
    version: 9.0.3
generated: "2021-08-10T18:24:15.569649-05:00"
```  
### Revisar un chart repository publico  
En algun navegador abrir la siguiente liga [https://charts.jenkins.io/index.yaml](https://charts.jenkins.io/index.yaml)  
  
Instalar wordpress  
```
helm install <NAME> stable/wordpress
helm status <NAME>
kubectl get pods
kubectl get svc
helm uninstall <NAME>
```  
