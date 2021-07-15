# kubernetes-openvpn
This repository deploys a openVPN server on kubernetes.

## Introduction
Deployment is based on suda helm chart (https://github.com/suda/charts) and its personal-openvpn chart (https://github.com/suda/charts/tree/main/charts/personal-ovpn)

## Deployment
### 1. Add suda helm chart
`helm repo add suda https://suda.github.io/charts/`

### 2. Install personal-openvpn chart
```
helm repo update
kubectl create namespace openvpn
helm install openvpn suda/personal-ovpn -f values.yaml
```

### 3. Generate secrets
```
export VPN_HOSTNAME=vpn.example.com
export DNS_SERVER=8.8.8.8
cd charts
# Generate basic OpenVPN config
./bin/generate-config
# Repeat this step for all the clients you need
CLIENT_NAME=my-client ./bin/add-client
# Set the Kubernetes secrets. Prepend with REPLACE=true to update existing ones
NAMESPACE=openvpn ./bin/set-secrets
```

