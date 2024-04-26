# airflow-k8s
Running Airflow Inside k8s

### Installing minikube
Visit https://minikube.sigs.k8s.io/docs/start/

#### Start 3 node cluster with cpu and ram settings
minikube start --nodes 3 -p mini-af --memory 16384 --cpus 4

#### Set current profile to mini-af
minikube profile mini-af

#### To view the cluster dashboard
minikube dashboard

#### To pull Airflow, we first need helm installed. Use below for windows, otherwise refer https://helm.sh/docs/intro/install/
winget install Helm.Helm