# Kubernetes Documentation #

## Features

- Prerequisites
- Installation
- Start Virtual Machine
- Basic Commands
- Advanced Commands

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)

## Installation

- [Kubernates](https://kubernetes.io/releases/download/)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)

Let's start by installing the above mentioned tools.

### Kubernates

1. First download the kubectl binary using the following command
    ```bash
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    ```
2. Download the checksum file
    ```bash
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
    ```
3. Verify the checksum
    ```bash
    echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
    ```
    Output should be like this
    ```bash
    kubectl: OK
    ```
4. Install the kubectl binary
    ```bash
    sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
    ```
5. Verify the installation
    ```bash
    kubectl version --client
    ```
    
### Minikube

1. Download the minikube binary
    ```bash
    curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
    ```
2. Install the minikube binary
    ```bash
    sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
    ```
3. Verify the installation
    ```bash
    minikube version
    ```
    
## Start Virtual Machine

| Command | Description |
| --- | --- |
| `minikube start` | Start the minikube virtual machine |
| `minikube status` | Check the status of the minikube virtual machine |
| `minikube ssh` | SSH into the minikube virtual machine |
| `minikube stop` | Stop the minikube virtual machine |
| `minikube delete` | Delete the minikube virtual machine |
| `minikube ip` | Get the IP address of the minikube virtual machine |
| `minikube dashboard --url` | Get the minikube dashboard URL |

- For more commands, refer to the [Minikube Documentation](https://minikube.sigs.k8s.io/docs/commands/)

## Basic Commands for Kubernetes

### Pods

| Command | Description |
| --- | --- |
| `kubectl get pods or kubectl get po or kubectl get pod` | List all pods in the current namespace |
| `kubectl get pods -n <namespace>` | List all pods in the specified namespace |
| `kubectl describe pod <pod-name>` | Describe a specific pod |
| `kubectl delete pod <pod-name>` | Delete a specific pod |
| `kubectl logs <pod-name>` | Get the logs of a specific pod |
| `kubectl exec -it <pod-name> -- /bin/bash` | Execute a command in a specific pod |

### Deployments

| Command | Description |
| --- | --- |
| `kubectl get deployments or kubectl get deploy` | List all deployments in the current namespace |
| `kubectl get deployments -n <namespace>` | List all deployments in the specified namespace |
| `kubectl describe deployment <deployment-name>` | Describe a specific deployment |
| `kubectl delete deployment <deployment-name>` | Delete a specific deployment |
| `kubectl scale deployment <deployment-name> --replicas=<number>` | Scale a specific deployment |

### Services

| Command | Description |
| --- | --- |
| `kubectl get services or kubectl get svc` | List all services in the current namespace |
| `kubectl get services -n <namespace>` | List all services in the specified namespace |
| `kubectl describe service <service-name>` | Describe a specific service |
| `kubectl delete service <service-name>` | Delete a specific service |

### Namespaces

| Command | Description |
| --- | --- |
| `kubectl get namespaces` | List all namespaces |
| `kubectl create namespace <namespace-name>` | Create a new namespace |
| `kubectl delete namespace <namespace-name>` | Delete a specific namespace |

- For more commands, refer to the [Kubernetes Documentation](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)

## Create objects using YAML files

### File Structure

```yaml
apiVersion: <APIVersion>
kind: <Kind>
metadata:
  name: <Name>
spec:
    <Key>: <Value>
```

### Examples

#### Pod

```yaml
apiVersion: v1                  # API Version
kind: Pod                       # Kind of object
metadata:                       
  name: nginx-pod               # Name for the pod
spec:
    containers:
    - name: nginx               # Name for the container
        image: nginx:latest     # Image for the container
    - limits:
        memory: "128Mi"         # Memory limit for the container
        cpu: "500m"             # CPU limit for the container
```

#### Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
    replicas: 3                 # Number of replicas
    selector:
        matchLabels:
            app: nginx          # Label for the deployment
    template:
        metadata:
            labels:
                app: nginx      # Label for the pod
        spec:
            containers:
            - name: nginx
                image: nginx:latest
                ports:
                - containerPort: 80
            - limits:
                memory: "128Mi"
                cpu: "500m"
```

#### Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
    selector:
        app: nginx
    ports:
    - protocol: TCP
        port: 80            # Port of the service
        targetPort: 80      # Port of the pod
    type: LoadBalancer
```

### Apply YAML files

```bash
kubectl apply -f <filename>.yaml
```

## Advanced Commands for Kubernetes

### Labels

| Command | Description |
| --- | --- |
| `kubectl label pods <pod-name> <key>=<value>` | Add a label to a specific pod |
| `kubectl label pods <pod-name> <key>-` | Remove a label from a specific pod |
| `kubectl get pods -l <key>=<value>` | List all pods with a specific label |

### Annotations

| Command | Description |
| --- | --- |
| `kubectl annotate pods <pod-name> <key>=<value>` | Add an annotation to a specific pod |
| `kubectl annotate pods <pod-name> <key>-` | Remove an annotation from a specific pod |
| `kubectl describe pod <pod-name>` | Describe a specific pod with annotations |

### ConfigMaps

| Command | Description |
| --- | --- |
| `kubectl create configmap <configmap-name> --from-file=<path>` | Create a configmap from a file |
| `kubectl get configmaps` | List all configmaps in the current namespace |
| `kubectl describe configmap <configmap-name>` | Describe a specific configmap |
| `kubectl delete configmap <configmap-name>` | Delete a specific configmap |

### Secrets

| Command | Description |
| --- | --- |
| `kubectl create secret generic <secret-name> --from-literal=<key>=<value>` | Create a secret from a literal |
| `kubectl get secrets` | List all secrets in the current namespace |
| `kubectl describe secret <secret-name>` | Describe a specific secret |
| `kubectl delete secret <secret-name>` | Delete a specific secret |

### Volumes

| Command | Description |
| --- | --- |
| `kubectl create -f <volume-definition.yaml>` | Create a volume from a definition file |
| `kubectl get pv` | List all persistent volumes |
| `kubectl get pvc` | List all persistent volume claims |
| `kubectl describe pv <pv-name>` | Describe a specific persistent volume |

### Helm

| Command | Description |
| --- | --- |
| `helm install <release-name> <chart-name>` | Install a Helm chart |
| `helm list` | List all Helm releases |
| `helm status <release-name>` | Get the status of a specific release |
| `helm uninstall <release-name>` | Uninstall a specific release |

- For more commands, refer to the [Helm Documentation](https://helm.sh/docs/helm/)

## Conclusion

This guide covers the basic and advanced commands for working with Kubernetes. It is recommended to explore the official documentation for more in-depth knowledge and understanding of Kubernetes.

## References

- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
- [Minikube Documentation](https://minikube.sigs.k8s.io/docs/)
- [Helm Documentation](https://helm.sh/docs/helm/)



