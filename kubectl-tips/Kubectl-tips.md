### kubectl tips  
- Creacion de objetos  
- Busqueda de recursos  
- Actualizacion de recursos  
- Borrando recursos  
- Interactuando con recursos  
- Obteniendo ayuda  

#### Creacion de objetos  

```
kubectl create ns <NAME>

cat << EOF | kubectl apply -n <NAME> -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment  
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

kubectl apply -f nginx.yaml -n <NAME>

kubectl create deployment nginx -n <NAME> --image=nginx:1.14.2 --dry-run=client -o yaml > name.yaml
```  

#### Busqueda de recursos  
```
# Lista todos los servicios en el namespace
kubectl get services -n <NAME>

# Lista todos los pods en todos los namespaces
kubectl get pods -A

# Lista todos los pods en el namespace con mas detalles
kubectl get pods -o wide -n <NAME>

# Lista los pods ordenados por nombre  
kubectl get services --sort-by=.metadata.name -n <NAME>
kubectl get svc --help

JSONPath
kubectl get pods POD_NAME -n <NAME> -o json | jq

{
	"apiVersion": "v1",
	"kind": "Pod",
	"metadata": {
		"annotations": {
			"cni.projectcalico.org/podIP": "192.168.16.139/32",
			"cni.projectcalico.org/podIPs": "192.168.16.139/32"
		},
		"creationTimestamp": "2021-07-23T00:06:23Z",
		"generateName": "nginx-deployment-66b6c48dd5-",
		"labels": {
			"app": "nginx",
			"pod-template-hash": "66b6c48dd5"
		},
		"name": "nginx-deployment-66b6c48dd5-7nc8r",
		"namespace": "diegoazd",
		"resourceVersion": "26536",
		"uid": "a9924420-9e55-4ced-be9d-392fb16d52e2"
	},
	"spec": {
		"containers": [{
			"image": "nginx:1.14.2",
			"imagePullPolicy": "IfNotPresent",
			"name": "nginx",
			"ports": [{
				"containerPort": 80,
				"protocol": "TCP"
			}]
		}],
		"dnsPolicy": "ClusterFirst",
		"enableServiceLinks": true,
		"nodeName": "fa866e85e71c.mylabserver.com",
		"preemptionPolicy": "PreemptLowerPriority",
		"priority": 0,
		"restartPolicy": "Always",
		"schedulerName": "default-scheduler",
		"securityContext": {},
		"serviceAccount": "default",
		"serviceAccountName": "default",
		"terminationGracePeriodSeconds": 30
	},
	"status": {
		"conditions": [{
				"lastProbeTime": null,
				"lastTransitionTime": "2021-07-23T00:06:24Z",
				"status": "True",
				"type": "Initialized"
			},
			{
				"lastProbeTime": null,
				"lastTransitionTime": "2021-07-23T00:06:26Z",
				"status": "True",
				"type": "Ready"
			},
			{
				"lastProbeTime": null,
				"lastTransitionTime": "2021-07-23T00:06:26Z",
				"status": "True",
				"type": "ContainersReady"
			},
			{
				"lastProbeTime": null,
				"lastTransitionTime": "2021-07-23T00:06:23Z",
				"status": "True",
				"type": "PodScheduled"
			}
		],
		"containerStatuses": [{
			"containerID": "containerd://3b22fca55102326cf8b8b23ed8594ff78f37ec0ce05e08210dba54909055f76b",
			"image": "docker.io/library/nginx:1.14.2",
			"imageID": "docker.io/library/nginx@sha256:f7988fb6c02e0ce69257d9bd9cf37ae20a60f1df7563c3a2a6abe24160306b8d",
			"lastState": {},
			"name": "nginx",
			"ready": true,
			"restartCount": 0,
			"started": true,
			"state": {
				"running": {
					"startedAt": "2021-07-23T00:06:25Z"
				}
			}
		}]
	}
}

Con el JSON anterior
kubectl get pods POD_NAME -n <NAME> -o jsonpath='{.kind}'
kubectl get pods POD_NAME -o=jsonpath='{.spec.containers[0]}' -n <NAME> | jq
kubectl get pods POD_NAME -o=jsonpath='{.status.conditions[*]}' -n <NAME> | jq
kubectl get pods POD_NAME -o=jsonpath='{.status.conditions[?(@.type!="Ready")]}' -n <NAME> | jq

# Mostar todos los pods filtrando por label app=nginx
kubectl get pods --selector=app=nginx -n <NAME>

kubectl get pods --show-labels -n <NAME>
kubectl label pods POD_NAME version=v1 -n <NAME>  
kubectl get pods --selector='!version' -n <NAME>

#Ver todos los eventos en todos los namespaces
kubectl get events --sort-by=.metadata.creationTimestamp -A
```  

Actualizacion de recursos
```
kubectl set image deployment/nginx-deployment nginx=nginx:1.14.1 -n <NAME>
kubectl get pods -o jsonpath='{.items[*].spec.containers[*].image}' -n <NAME>

kubectl rollout history deployment/nginx-deployment -n <NAME>
kubectl rollout undo deployment/nginx-deployment -n <NAME>
kubectl get pods -o jsonpath='{.items[*].spec.containers[*].image}' -n <NAME>
kubectl rollout restart deployment/nginx-deployment -n <NAME>               
kubectl get pods -n <NAME>

kubectl edit deployment nginx-deployment -n <NAME>
```  

Interactuando con pods  
```
kubectl logs POD_NAME -n <NAME>
kubectl logs -l app=nginx -n <NAME>
kubectl logs -l app=nginx -c nginx -n <NAME>


kubectl port-forward pod-name 8000:80 -n <NAME>
kubectl exec POD_NAME -n <NAME> -- cat /etc/nginx/nginx.conf
kubectl exec -it POD_NAME -n <NAME>- - /bin/bash
```

Borrando recursos  
```
kubectl delete deployment nginx-deployment -n <NAME>
```  

Obteniendo ayuda  
```
kubectl <CMD> --help
kubectl api-resources
kubectl api-resources --namespaced=true      # All namespaced resources
kubectl api-resources --namespaced=false     # All non-namespaced resources
kubectl api-resources -o name                # All resources with simple output (only the resource name)
kubectl api-resources -o wide                # All resources with expanded (aka "wide") output
kubectl api-resources --verbs=list,get       # All resources that support the "list" and "get" request verbs
kubectl api-resources --api-group=extensions # All resources in the "extensions" API group

kubectl explain <NAME> --recursive
```
