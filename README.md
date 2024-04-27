# airflow-k8s
Running Airflow Inside k8s

### Installing minikube
Visit https://minikube.sigs.k8s.io/docs/start/

#### Start 3 node cluster with cpu and ram settings
minikube start --nodes 3 -p mini-af --memory 16384 --cpus 4

#### Set current profile to mini-af
minikube profile mini-af

You may have to rename the path C:\Users\{username}}\.docker\contexts\meta\37a8eec1ce19687d132fe29051dca629d164e2c...longstring/meta.json

#### To view the cluster dashboard
minikube dashboard

#### To pull Airflow, we first need helm installed. Use below for windows, otherwise refer https://helm.sh/docs/intro/install/
winget install Helm.Helm

#### Airflow install
Before installing Airflow make sure the default context is mini-af

C:\Users\vikas\Documents\github\airflow-k8s>kubectl config current-context
mini-af

#### Add official helm chart to repo
helm repo update
helm repo add apache-airflow https://airflow.apache.org

#### Export the official values to values.yml
helm show values apache-airflow/airflow >values.yml

added the below section to postgres to overcome PVC issues seen during install
postgresql:
  volumePermissions:
     enabled: true

#### install to namespace airflow
helm install -f values.yml --kube-context mini-af airflow apache-airflow/airflow -n airflow --create-namespace

#### Once it starts portforward to host
kubectl port-forward svc/airflow-webserver 8080:8080 --namespace airflow &
START "" kubectl port-forward svc/airflow-webserver 8080:8080

helm uninstall airflow
kubectl delete namespace airflow

#### Other helpful commands
kubectl cluster-info
minikube status -w
kubectl get nodes



1. https://chocolatey.org/install
2. choco install kind
Installed:
 - chocolatey-dotnetfx.extension v1.0.1
 - docker-desktop v4.29.0
 - dotnetfx v4.8.0.20220524
 - KB2919355 v1.0.20160915
 - KB2919442 v1.0.20160915
 - kind v0.22.0

3. helm repo add apache-airflow https://airflow.apache.org
   helm install airflow apache-airflow/airflow --namespace airflow --debug
4. helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
   helm upgrade --install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --create-namespace --namespace kubernetes-dashboard
   kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard-kong-proxy 8443:443
5. kubectl apply -f service-account.yml
   kubectl -n kubernetes-dashboard create token admin-user
6. kubectl describe serviceaccount admin-user -n kubernetes-dashboard

eyJhbGciOiJSUzI1NiIsImtpZCI6ImZyM1Bob19HYjBqbGdXRkxoY2tVSVk4TzE3SWhDYkJmbjlBU0l3NTd5Z0kifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNzE0MjYxMDE1LCJpYXQiOjE3MTQyNTc0MTUsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsInNlcnZpY2VhY2NvdW50Ijp7Im5hbWUiOiJhZG1pbi11c2VyIiwidWlkIjoiZTc5ZGFhZjItZjg1ZS00NTcyLTlhM2EtNjRjYjE5NGE0OTNkIn19LCJuYmYiOjE3MTQyNTc0MTUsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlcm5ldGVzLWRhc2hib2FyZDphZG1pbi11c2VyIn0.UC8_MwU7O70FYP7XHderWTjdAyW1t30P_5nd0Olxps1uGhk_CI1wLkn0xaj4fdu03eKrpCM14HFrEgQ0pccKrFxvv3ucQm1tEhOClIci9i1EFEWZiObJAGS1Q_bZJ3bHvhECXbhU2fGBz4XyJuP_YMXByEEFL7DUJ6x5LKceNIHszQiGySMAwGqCim5tUsNff09w7f3x8f45WkmMh-lqRCCPb-uykDgByL5HKqVLHIrYP1mjWiO57MdOd-vVtdaTPCbrwhbDVk5g0i1Dzuw8gpznaC9CdUnFwanUqNSf2Fn-KBSCv85Zj5wXfkX8H7Yi5L1QuEcjKZswcKqzKYKqVA

7. helm uninstall airflow
8. kubectl delete namespace airflow

9. helm install -f values_kind.yml --kube-context kind-airflow-cluster airflow apache-airflow/airflow -n airflow --create-namespace