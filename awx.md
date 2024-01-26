


### Minikube Installation:

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
minikube version

```

### Minikube Start:

```
minikube start --cpus=4 --memory=6g --addons=ingress
minikube status
docker ps -a
minikube kubectl -- get nodes
``` 


### AWX Operator:

```
## vim kustomization.yaml

apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - github.com/ansible/awx-operator/config/default?ref=2.0.1
images:
  - name: quay.io/ansible/awx-operator
    newTag: 2.0.1
namespace: awxInstallation:


## kubectl apply -k.
## kubectl get pods -n awx

NAME                                              READY   STATUS    RESTARTS   AGE
awx-operator-controller-manager-c7795b96b-2m84d   2/2     Running   0          3m33s


##cat awx-server.yaml

---
apiVersion: awx.ansible.com/v1beta1
kind: AWX
metadata:
  name: awx-server
spec:
  service_type: nodeport


## cat kustomization.yaml

---
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - github.com/ansible/awx-operator/config/default?ref=2.0.1
  - awx-server.yaml
images:
  - name: quay.io/ansible/awx-operator
    newTag: 2.0.1
namespace: awx


## kubectl apply -k.

```


### Accessing AWX:


```
## kubectl config set-context --current --namespace=awx
Context "minikube" modified.


## kubectl get pods
NAME                                               READY   STATUS    RESTARTS   AGE
awx-operator-controller-manager-6c5bb555b6-hp4xl   2/2     Running   0          6m8s
awx-server-postgres-13-0                           1/1     Running   0          98s

## kubectl get pods
NAME                                               READY   STATUS    RESTARTS      AGE
awx-operator-controller-manager-6c5bb555b6-hp4xl   2/2     Running   2 (48s ago)   13m
awx-server-postgres-13-0                           1/1     Running   0             9m12s
awx-server-task-688bc87d96-vg9d2                   4/4     Running   0             7m32s
awx-server-web-64659d599d-znmqd                    3/3     Running   0             5m24s

## kubectl get svc
NAME                                              TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
awx-operator-controller-manager-metrics-service   ClusterIP   10.105.211.112   <none>        8443/TCP       16m
awx-server-postgres-13                            ClusterIP   None             <none>        5432/TCP       11m
awx-server-service                                NodePort    10.103.229.150   <none>        80:30826/TCP   10m
 
 
## minikube ip
192.168.39.20

## kubectl get svc awx-server-service
NAME                 TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
awx-server-service   NodePort   10.103.229.150   <none>        80:30826/TCP   17m


http://192.168.39.20:30826

```


### Minikube Dashboard:
```
minikube dashboard
```



### Here are the steps for checking the admin password from the minikube datasbord.

```
1). Select namespace awx
2). Click on secret key
3). Click on awx-server-admin-password
4). Now, click on the password from data.
```


