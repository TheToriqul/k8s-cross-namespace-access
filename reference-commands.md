# Kubernetes Cross-Namespace Communication Command Reference Guide

### Project Content Table
- [Section 1: Core Setup Commands](#section-1-core-setup-commands)
- [Section 2: Deployment Operations](#section-2-deployment-operations)
- [Section 3: Verification and Testing](#section-3-verification-and-testing)
- [Section 4: Cleanup Operations](#section-4-cleanup-operations)

> **Author**: [Md Toriqul Islam](https://linkedin.com/in/thetoriqul)  
> **Description**: Comprehensive command reference for implementing cross-namespace communication in Kubernetes  
> **Learning Focus**: Kubernetes networking, namespace management, and service discovery  
> **Note**: Review each command carefully before execution in your environment.

## Section 1: Core Setup Commands

### Step 1: Environment Preparation
```bash
# Install required tools
sudo apt update
sudo apt install vim -y

# Verify kubectl installation
kubectl version --client

# Create required namespaces
kubectl create namespace namespace-a
kubectl create namespace namespace-b

# Verify namespace creation
kubectl get namespaces
```

### Step 2: Configuration File Creation
```bash
# Create Nginx deployment configuration
vim nginx-deployment.yaml

# Create Nginx service configuration
vim nginx-service.yaml

# Create BusyBox pod configuration
vim busybox.yaml

# Verify configuration files
ls -l *.yaml
```

## Section 2: Deployment Operations

### Nginx Deployment
```bash
# Deploy Nginx in namespace-a
kubectl apply -f nginx-deployment.yaml

# Deploy Nginx service
kubectl apply -f nginx-service.yaml

# Verify Nginx deployment
kubectl get deployments -n namespace-a
kubectl get pods -n namespace-a
kubectl get services -n namespace-a

# Check Nginx pod logs
kubectl logs -n namespace-a -l app=nginx
```

### BusyBox Deployment
```bash
# Deploy BusyBox in namespace-b
kubectl apply -f busybox.yaml

# Verify BusyBox deployment
kubectl get pods -n namespace-b

# Check BusyBox pod status
kubectl describe pod busybox -n namespace-b
```

## Section 3: Verification and Testing

### Cross-Namespace Communication Testing
```bash
# Access BusyBox pod shell
kubectl exec -it busybox -n namespace-b -- sh

# Test DNS resolution
nslookup nginx-service.namespace-a.svc.cluster.local

# Test HTTP connection
wget -qO- http://nginx-service.namespace-a.svc.cluster.local

# Exit BusyBox shell
exit

# Verify service endpoints
kubectl get endpoints -n namespace-a nginx-service
```

### Monitoring and Debugging
```bash
# Monitor pod status
kubectl get pods -n namespace-a -w
kubectl get pods -n namespace-b -w

# Check service details
kubectl describe service nginx-service -n namespace-a

# View pod logs
kubectl logs -f -n namespace-a -l app=nginx
```

## Section 4: Cleanup Operations

### Resource Cleanup
```bash
# Remove deployments and services
kubectl delete -f nginx-deployment.yaml
kubectl delete -f nginx-service.yaml
kubectl delete -f busybox.yaml

# Delete namespaces (caution: removes all resources in namespaces)
kubectl delete namespace namespace-a
kubectl delete namespace namespace-b

# Verify cleanup
kubectl get all -n namespace-a
kubectl get all -n namespace-b
```

## Learning Notes

1. Kubernetes DNS format: `<service-name>.<namespace>.svc.cluster.local`
2. Cross-namespace communication requires proper service exposure
3. Always verify service endpoints before testing connectivity
4. Use appropriate namespace contexts for commands
5. Monitor logs for troubleshooting connection issues

---

> 💡 **Best Practice**: Always verify resource creation in the correct namespace before testing connectivity

> ⚠️ **Warning**: Deleting namespaces will remove ALL resources within them

> 📝 **Note**: Keep service names consistent across configurations for easier maintenance