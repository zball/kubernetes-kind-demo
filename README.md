To build the Docker image:
```bash
docker build --tag themessaround/simplego:v1.0 .
```

Start the Kind Cluster:
```bash
kind create cluster --config k8s/kind-config.yaml  --name simplego-cluster
```

Load the built image: (this needs done after every build)
```sh
kind load docker-image themessaround/simplego:v1.0 --name simplego-cluster 
```

Apply Kubernetes config:
```sh
kubectl apply -k k8s 
```

Verify pods are running:
```sh
kubectl get pods --namespace=simplego 
```

Check logs:
```sh
kubectl logs -l app=simplego --all-containers -f --namespace=simplego
```

At this point should be able to hit the simplego service:
```sh
curl localhost/hello
```

## Making updates

Rebuild the image:
```sh
docker build --tag themessaround/simplego:v1.0 .
```

Reload the image:
```sh
kind load docker-image themessaround/simplego:v1.0 --name simplego-cluster 
```

Rollout changes:
```sh
kubectl rollout restart deployment simplego-pod --namespace=simplego
```