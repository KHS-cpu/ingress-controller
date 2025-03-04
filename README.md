# Ingress-controller
# An Ingress Controller in Kubernetes is used to route external traffic to multiple services running inside the cluster. Instead of exposing each service with a separate LoadBalancer, the Ingress Controller acts as a reverse proxy, allowing you to define rules for routing traffic based on path or hostname. This reduces the need for multiple external IPs and helps you expose only the necessary services to the public while keeping others internal.

# Install ingress controller with helm
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install nginx-ingress ingress-nginx/ingress-nginx --namespace default

# Install configmap.yaml & nginx-config.yaml first then install deployment.yaml & service.yaml
# Install ingress-resource.yaml file to put ingress-resource to ingress controller.
# Check the ingress controller get external IP by using
kubectl get svc

# Apply that IP to host /etc/hosts path
echo "172.18.255.151 www.khs-lab" | sudo tee -a /etc/hosts > /dev/null  # Since this is lab environment I use this as local DNS

# Test if it is working or not
curl http://www.khs-lab/html

# You can also test if it is working in browser also
