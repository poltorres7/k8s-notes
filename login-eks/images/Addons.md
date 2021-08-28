### Instalacion addons de istio  
Los addons a instalar son los siguientes:  
- Prometheus  
- Grafana  
- Jaeger  
- Kiali  

#### Prometheus  
Lo primero es crear un grupo de nodos para instalar prometheus  
`eksctl create nodegroup --config-file=eks-ng-prometheus.yaml`

La instalacion de prometheus se baso en la siguiente guia de [istio](https://istio.io/latest/docs/ops/integrations/prometheus/)  
Instalacion  
Ir a istio/addons/prometheus ejecutar el siguiente comando  
`kubectl apply -f prometheus.yaml`  

#### Grafana  

La instalacion se basa en la siguiente [documentacion](https://istio.io/latest/docs/ops/integrations/grafana/)  
Ir a istio/addons/grafana  
```
kubectl apply -f grafana-cred.yaml
kubectl apply -f grafana.yaml
```   
Nota:  
La instalacion usa keycloak por lo tanto hay que agregar el dominio `grafana.telemetry.staging.kubofinanciero.app` al client grafana  

#### Jaeger  
La instalacion se basa en la siguiente [documentacion](https://istio.io/latest/docs/ops/integrations/jaeger/)  
Ir a istio/addons/jaeger y ejecutar los siguientes comandos  
```
kubectl apply -f jaeger-cred.yaml
kubectl apply -f jaeger.yaml
```  
Nota:  
La instalacion usa keycloak por lo tanto hay que agregar el dominio `jaeger.telemetry.staging.kubofinanciero.app` al client Jaeger-proxy    

#### Kiali  
La instalacion se basa en la siguiente [documentacion](https://istio.io/latest/docs/ops/integrations/kiali/)  
Ir a istio/addons/jaeger y ejecutar los siguientes comandos  
```
kubectl apply -f kiali-creds.yaml
kubectl apply -f kiali.yaml
```  
Nota:  
La instalacion usa keycloak por lo tanto hay que agregar el dominio `kiali.telemetry.staging.kubofinanciero.app` al client Kiali  
