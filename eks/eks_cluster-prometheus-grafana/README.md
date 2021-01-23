# Creating eks cluster, installing prometheus, graphana
## Prerequisites:
* install awscli, eksli, kubectl, helm
## Create EKS cluster
~~~
eksctl create cluster -f eks_cluster.yaml
~~~
## Configure the Helm
~~~
helm init
helm repo add stable https://charts.helm.sh/stable
helm repo update
~~~
## Deploy Prometheus by by helm
~~~
kubectl create namespace prometheus
helm install stable/prometheus --namespace prometheus --set alertmanager.persistentVolume.storageClass="gp2" --set server.persistentVolume.storageClass="gp2" --generate-name
~~~
## Deploy Grafana
~~~
kubectl create namespace grafana
helm install stable/grafana --namespace grafana --set persistence.storageClassName="gp2" --set adminPassword=grafana --set service.type=LoadBalancer --generate-name
kubectl get svc -n grafana
~~~
## Set up Grafana
* Add you first data source -> Prometheus -> URL: <clusterIP of prometheus server> -> Save
![image](https://user-images.githubusercontent.com/53195216/105606365-01be3c00-5daa-11eb-804a-684d45f50b95.png)
* Click "+" then "Import" on the left panel -> set value for "Import via grafana.com": 10000 -> Select Prometheus value for Prometheus field -> Click Import
  ![image](https://user-images.githubusercontent.com/53195216/105606560-31217880-5dab-11eb-809d-5473fb719403.png)
