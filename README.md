# viktoriya666_platform
viktoriya666 Platform repository

HW2. Настройка локального окружения. Запуск первого контейнера. Работа с kubectl

- install kubectl for linux brew
- install minikube
- create minikube cluster
- creating a manifest pod with a docker image
- create mamifest web-pod.yaml
- see log pods
- search errors after running pod

HW3. Kubernetes controllers. ReplicaSet, Deployment, DaemonSet

- install kind and create kind cluster
- create manifest payment service and push on DockerHub
- create ReplicaSet, Deployment (RollingUpdate, BlueGreen strategy), DaemonSet
- add Probes
- create DaemonSet for Node Exporter


HW4. Security
- create user for namespace
- create namespace
- create Service Account
- add Roles for acciunts

HW6. Volumes, Storages, StatefulSet

- create Statefulset
- create Secret

HW8. Операторы,CustomResourceDefinition

- create minikube cluster
- create pod with MySql
- create database
- create Custom Resourse
- create CustomResourceDefinition
- insert data from table MySQL

command:
```
vika@vika:~/viktoriya666_platform/kubernetes-operators/deploy$ kubectl get jobs
```
answer:
```
vika@vika:~/viktoriya666_platform/kubernetes-operators/deploy$ kubectl get jobsNAME   COMPLETIONS   DURATION   AGE
backup-mysql-instance-job    1/1           15m        15m
restore-mysql-instance-job   1/1           32m        32m

```


command:
```
export MYSQLPOD=$(kubectl get pods -l app=mysql-instance -o jsonpath="
{.items[*].metadata.name}")
```
```
kubectl exec -it $MYSQLPOD -- mysql -potuspassword -e "select * from test;" otus-database
```

answer:
```
vika@vika:~/viktoriya666_platform/kubernetes-operators/deploy$ kubectl exec -it $MYSQLPOD -- mysql -potuspassword -e "select * from test;" otus-database
mysql: [Warning] Using a password on the command line interface can be insecure.
+----+-------------+
| id | name        |
+----+-------------+
|  1 | some data   |
|  2 | some data-2 |
+----+-------------+
```

HW8. Monitoring

start:

minikube start
kubectl create ns monitoring
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm search repo prometheus-community
helm install prometheus-operator prometheus-community/kube-prometheus-stack -n monitoring

kubectl --namespace monitoring get pods -l "release=prometheus-operator"
NAME                                                   READY   STATUS    RESTARTS   AGE
prometheus-operator-kube-p-operator-67c8b56d8c-rq6n6   1/1     Running   0          30s
prometheus-operator-prometheus-node-exporter-vjfhp     1/1     Running   0          30s

deploy:

kubectl apply -f nginx-configMap.yaml
kubectl apply -f nginx-deployment.yaml
kubectl apply -f nginx-service.yaml
kubectl apply -f nginx-serviceMonitor.yaml

results:

kubectl get pods -n monitoring
NAME                                                      READY   STATUS    RESTARTS   AGE
alertmanager-prometheus-operator-kube-p-alertmanager-0    2/2     Running   0          20m27s
nginx-75756558dd-8pjnc                                    2/2     Running   0          113s
nginx-75756558dd-vvc79                                    2/2     Running   0          113s
nginx-75756558dd-z6q4r                                    2/2     Running   0          113s
prometheus-operator-grafana-67ff95c8f5-qkcwl              2/2     Running   0          20m32s
prometheus-operator-kube-p-operator-67c8b56d8c-rq6n6      1/1     Running   0          20m32s
prometheus-operator-kube-state-metrics-69fcc8d48c-tr9ht   1/1     Running   0          20m32s
prometheus-operator-prometheus-node-exporter-vjfhp        1/1     Running   0          20m32s
prometheus-prometheus-operator-kube-p-prometheus-0        2/2     Running   1          20m27s
