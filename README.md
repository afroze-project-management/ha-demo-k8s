# Prerequisites

* Docker
* Kubernetes
* Minikube (with metrics-server plugin enabled)
* K6 (API Load Testing Tool)

# Usage

Create Docker Image:

```
docker build -t <repository_name>\ha-demo:latest
```

Push Docker Image to Docker Hub:

```
docker push <repository_name>\ha-demo:latest
```

Deploy to Kubernetes:

```
kubectl apply -f .\k8s.yml
```

Ensure Minikube is forwarding requests to the docker image:

```
minikube service ha-demo --url
```

This will give you a URL for the api e.g.:

```
http://127.0.0.1:25775
```

Open loadtest.js and replace url with the above url.

Open a new terminal and run:

```
k6 run --vus 10 --duration 120s .\loadtest.js
```

Here, we are configuring k6 to run with 10 simultaneous requests for a total duration of 120s.

The number of replica pods for the api can be seen by either using minikube dashboard or by running `kubectl get hpa --watch`