# Traefik-Ingress-Setup

Created by: Raihan Islam
Created time: March 26, 2024 11:21 AM

## **Step 1.1: Install Helm [ on Linux ]**

1. Adding the Helm repository:
    
    ```bash
    curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
    sudo apt-get install apt-transport-https --yes
    echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
    ```
    
2. Updating the package list:
    
    ```bash
    Update the package list
    ```
    
3. Installing helm:
    
    ```bash
    sudo apt-get install helm
    ```
    
4. Verifying that Helm is installed:
    
    ```bash
    helm version
    ```
    

## **Step 1.1: Install Helm [ on macOS ]**

1. Installing HomeBrew
    
    ```bash
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    ```
    
2. Installing helm using brew
    
    ```bash
    brew install helm
    ```
    

## **Step 2: Installing Traefik**

1. Install Traefik using Helm:
    
    ```bash
    helm repo add traefik https://helm.traefik.io/traefik
    helm repo update
    helm install traefik traefik/traefik
    ```
    

![traefik-installation.png](https://github.com/Raihan-009/kubernetes-learning-docs/blob/main/traefik-installtion/images/traefik-installation.png?raw=true)

## Step 2.1: Finding args of traefik pod

1. Get the list of pods
    
    ```bash
    kubectl get pods
    ```
    
2. Run the command to get full description of traefik pod
    
    ```bash
    kubectl describe pod traefik-cb6c6f8c7-5v2n2  
    ```
    
    ![arguments.png](https://github.com/Raihan-009/kubernetes-learning-docs/blob/main/traefik-installtion/images/arguments.png?raw=true)
    
    - `click`
        1. **`-entrypoints.metrics.address=:9100/tcp`**: This specifies the address for the metrics entrypoint, typically used by Prometheus for scraping metrics.
        2. **`-entrypoints.traefik.address=:9000/tcp`**: This specifies the address for the Traefik dashboard.
        3. **`-entrypoints.web.address=:8000/tcp`**: This specifies the address for the HTTP entrypoint.
        4. **`-entrypoints.websecure.address=:8443/tcp`**: This specifies the address for the HTTPS entrypoint.

## Step 2.2: Port forward to traefik pod

1. Port-forward to traefik pod
    
    ```bash
    kubectl port-forward pod/traefik-cb6c6f8c7-5v2n2 9001:9000
    ```
    
2. Accessing dashboard from browser
    
    ![dashboard.png](https://github.com/Raihan-009/kubernetes-learning-docs/blob/main/traefik-installtion/images/dashboard.png?raw=true)
    
3. For the http entrypoint [ http traffic to ingress controller ]
    
    ```bash
    kubectl port-forward pod/traefik-cb6c6f8c7-5v2n2 8001:8000
    ```
    
4. Accessing from local browser
    
    ![http-traffic.png](https://github.com/Raihan-009/kubernetes-learning-docs/blob/main/traefik-installtion/images/http-traffic.png?raw=true)
    

> Now, we need to set an ingress configuration which will be used to route HTTP traffic to a backend service.
> 
> 
> `ingress.yml`
> 
> ```yaml
> apiVersion: networking.k8s.io/v1
> kind: Ingress
> metadata:
>   name: nginx-ingress-test
> 
> spec:
>   rules:
>     - http:
>         paths:
>           - path: /
>             pathType: Exact
>             backend:
>               service:
>                 name: nginx-service
>                 port:
>                   number: 80
> ```
> 

1. Now again, accessing from browser
    
    ![nginx-8001.png](https://github.com/Raihan-009/kubernetes-learning-docs/blob/main/traefik-installtion/images/nginx-8001.png?raw=true)