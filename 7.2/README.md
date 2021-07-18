https://www.linuxtechi.com/setup-highly-available-kubernetes-cluster-kubeadm/
kubeadm reset

Kubeadm high availabilty

Ejecurtar el comando en cada nodo como se muestra en el siguiente ejemplo
sudo hostname "k8s-master-1"
exec bash


Editar /etc/hosts con todas las ips en los nodos maestros

172.31.96.170 k8s-master-1
172.31.97.161 k8s-master-2
172.31.106.69 k8s-worker-1
172.31.99.165 k8s-worker-2
172.31.112.198 vip-k8s-master


Paso 2: Instalar y configurar keepalive y ha proxy en todos los nodos master

sudo apt-get update
sudo apt-get install linux-headers-$(uname -r)
sudo apt install haproxy keepalived -y


sudo vim /etc/keepalived/check_apiserver.sh


#!/bin/sh
APISERVER_VIP=172.31.112.198
APISERVER_DEST_PORT=6443

errorExit() {
    echo "*** $*" 1>&2
    exit 1
}

curl --silent --max-time 2 --insecure https://localhost:${APISERVER_DEST_PORT}/ -o /dev/null || errorExit "Error GET https://localhost:${APISERVER_DEST_PORT}/"
if ip addr | grep -q ${APISERVER_VIP}; then
    curl --silent --max-time 2 --insecure https://${APISERVER_VIP}:${APISERVER_DEST_PORT}/ -o /dev/null || errorExit "Error GET https://${APISERVER_VIP}:${APISERVER_DEST_PORT}/"
fi



Dar permisos de ejecucion al archivo
sudo chmod +x /etc/keepalived/check_apiserver.sh

sudo sh -c '> /etc/keepalived/keepalived.conf'

Revisar usuario a ejecutar el archivo y la intarface de red
sudo vim /etc/keepalived/keepalived.conf

global_defs {
    router_id LVS_DEVEL
    script_user cloud_user
}
vrrp_script check_apiserver {
  script "/etc/keepalived/check_apiserver.sh"
  interval 3
  weight -2
  fall 10
  rise 2
}

vrrp_instance VI_1 {
    state MASTER
    interface ens5
    virtual_router_id 151
    priority 255
    authentication {
        auth_type PASS
        auth_pass P@##D321!
    }
    virtual_ipaddress {
        172.31.112.198/24
    }
    track_script {
        check_apiserver
    }
}

hacer lo misnmo en los otros nodos master pero cambiar state por SLAVE y bajar la prioridad


Configurar HAProxy

Respaldar configuracion actual
sudo cp /etc/haproxy/haproxy.cfg /etc/haproxy/haproxy.cfg-org


sudo vim /etc/haproxy/haproxy.cfg y agregar las siguientes lineas despues de la seccion global


frontend apiserver
    bind *:8443
    mode tcp
    option tcplog
    default_backend apiserver
#---------------------------------------------------------------------
# round robin balancing for apiserver
#---------------------------------------------------------------------
backend apiserver
    option httpchk GET /healthz
    http-check expect status 200
    mode tcp
    option ssl-hello-chk
    balance     roundrobin
        server k8s-master-1 172.31.96.170:6443 check
        server k8s-master-2 172.31.97.161:6443 check



Habilitar keepalived y haproxy


sudo service keepalived start
sudo service haproxy start

ip addr

 2: ens5: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc mq state UP group default qlen 1000
    link/ether 0a:0b:ea:5f:6a:15 brd ff:ff:ff:ff:ff:ff
    inet 172.31.107.49/20 brd 172.31.111.255 scope global dynamic ens5
       valid_lft 3064sec preferred_lft 3064sec
    inet 172.31.112.198/24 scope global ens5
       valid_lft forever preferred_lft forever
    inet6 2600:1f18:502:2f01:37e0:34af:f7d1:3a4b/128 scope global dynamic noprefixroute
       valid_lft 436sec preferred_lft 136sec
    inet6 fe80::80b:eaff:fe5f:6a15/64 scope link
       valid_lft forever preferred_lft forever

---

Preparacion del entorno:

Esto hay que hacerse en todos los servidores:


Crear configuracion de containerd
cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF

Cargar modulos:
sudo modprobe overlay
sudo modprobe br_netfilter


Agregan configuracion para kubernetes networking:
cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF

Aplican los cambios:
sudo sysctl --system

Instalar containerd:
sudo apt-get update && sudo apt-get install -y containerd

Crear configuracion para containerd
sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml


Reiniciar y verificar estatus:
sudo systemctl restart containerd
sudo systemctl status containerd

Deshabilitar el swap:
sudo swapoff -a

Deshabilita al iniciar la maquina"
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

Instalan dependencias:
sudo apt-get update && sudo apt-get install -y apt-transport-https curl

Descargar paquetes de kubernetes y agregar kubernetes a la lista de repositorios:
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF


Instalar kubernetes
sudo apt-get update
sudo apt-get install -y kubelet=1.21.0-00 kubeadm=1.21.0-00 kubectl=1.21.0-00
Deshabilita los siguientes paquetes para que no actualicen automaticamente
sudo apt-mark hold kubelet kubeadm kubectl


Inicializar el cluster de kuberntes solo en el nodo 1

sudo kubeadm init --control-plane-endpoint "vip-k8s-master:8443" --upload-certs

Copiar la salida del comando ya que se usaran los comandos posteriormente:

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of the control-plane node running the following command on each as root:

  kubeadm join vip-k8s-master:8443 --token ghwqno.og72izso3vjcdhgt \
        --discovery-token-ca-cert-hash sha256:4881de9af021a250d1becbcdab35ba37dca6ab05a079f7d505b95c70b688a9f1 \
        --control-plane --certificate-key bf3819d98e3f456c2f8eaf0891c62f3fb42549c82372c11fe4eb6b545fd5e74a

Please note that the certificate-key gives access to cluster sensitive data, keep it secret!
As a safeguard, uploaded-certs will be deleted in two hours; If necessary, you can use
"kubeadm init phase upload-certs --upload-certs" to reload certs afterward.

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join vip-k8s-master:8443 --token ghwqno.og72izso3vjcdhgt \
        --discovery-token-ca-cert-hash sha256:4881de9af021a250d1becbcdab35ba37dca6ab05a079f7d505b95c70b688a9f1




Inicializar configuracion con kubectl:
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl get nodes
NAME           STATUS     ROLES                  AGE    VERSION
k8s-master-1   NotReady   control-plane,master   2m7s   v1.21.0


Inicializar calico manifest
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml

 kubectl get pods -A


kubectl get nodes
NAME           STATUS     ROLES                  AGE    VERSION
k8s-master-1   Ready   control-plane,master   3m4s   v1.21.0

En los servidores master ejecutar el comando que nos mostro al inicializar cluster

sudo kubeadm join vip-k8s-master:8443 --token ghwqno.og72izso3vjcdhgt \
        --discovery-token-ca-cert-hash sha256:4881de9af021a250d1becbcdab35ba37dca6ab05a079f7d505b95c70b688a9f1 \
        --control-plane --certificate-key bf3819d98e3f456c2f8eaf0891c62f3fb42549c82372c11fe4eb6b545fd5e74a



Instalacion load balancer:
sudo apt install -y nginx

Editar el archiv /etc/nginx/nginx.conf

events { }

stream {
    upstream stream_backend {
        least_conn;
        # REPLACE WITH master1 IP
        server 172.31.96.170:8443;
        # REPLACE WITH master2 IP
        server 172.31.97.161:8443;

    }

    server {
        listen        8443;
        proxy_pass    stream_backend;
        proxy_timeout 3s;
        proxy_connect_timeout 1s;
    }

}

sudo service nginx start

----
Configuracion worker nodes:

Editar el archivo /etc/hosts
172.31.106.105 k8s-master-1
172.31.107.49 k8s-master-2
172.31.105.166 k8s-worker-1
172.31.98.12 k8s-worker-2
172.31.97.142 vip-k8s-master



sudo kubeadm join vip-k8s-master:8443 --token ghwqno.og72izso3vjcdhgt \
        --discovery-token-ca-cert-hash sha256:4881de9af021a250d1becbcdab35ba37dca6ab05a079f7d505b95c70b688a9f1



Listar contenedores corriendo en nodo:

ps aux | grep containerd
sudo ctr -n k8s.io c list
