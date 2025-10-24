# Kubernetes Commands Cheat Sheet

## Table of Contents
- [Cluster Management](#cluster-management)
- [Nodes](#nodes)
- [Namespaces](#namespaces)
- [Pods](#pods)
- [Deployments](#deployments)
- [Services](#services)
- [ConfigMaps & Secrets](#configmaps--secrets)
- [Persistent Volumes](#persistent-volumes)
- [StatefulSets](#statefulsets)
- [DaemonSets](#daemonsets)
- [Jobs & CronJobs](#jobs--cronjobs)
- [Ingress](#ingress)
- [Resource Management](#resource-management)
- [Logs & Debugging](#logs--debugging)
- [Security & RBAC](#security--rbac)
- [Networking](#networking)
- [Helm](#helm)
- [kubectl Tips & Tricks](#kubectl-tips--tricks)

---

## Cluster Management

### Cluster Information
```bash
# Display cluster info
kubectl cluster-info

# Display cluster info with detailed output
kubectl cluster-info dump

# Get cluster version
kubectl version

# Get cluster configuration
kubectl config view

# Get current context
kubectl config current-context

# List all contexts
kubectl config get-contexts

# Switch context
kubectl config use-context [CONTEXT_NAME]

# Set default namespace for context
kubectl config set-context --current --namespace=[NAMESPACE]

# Rename context
kubectl config rename-context [OLD_NAME] [NEW_NAME]

# Delete context
kubectl config delete-context [CONTEXT_NAME]
```

### API Resources
```bash
# List all API resources
kubectl api-resources

# List API resources with short names
kubectl api-resources --namespaced=true

# List API versions
kubectl api-versions

# Explain resource and its fields
kubectl explain [RESOURCE]

# Explain specific field
kubectl explain [RESOURCE].[FIELD]
```

---

## Nodes

### Node Operations
```bash
# List all nodes
kubectl get nodes

# Describe a node
kubectl describe node [NODE_NAME]

# Get node details in YAML
kubectl get node [NODE_NAME] -o yaml

# Get node details in JSON
kubectl get node [NODE_NAME] -o json

# Label a node
kubectl label node [NODE_NAME] [KEY]=[VALUE]

# Remove label from node
kubectl label node [NODE_NAME] [KEY]-

# Taint a node
kubectl taint node [NODE_NAME] [KEY]=[VALUE]:[EFFECT]

# Remove taint from node
kubectl taint node [NODE_NAME] [KEY]:[EFFECT]-

# Cordon a node (mark as unschedulable)
kubectl cordon [NODE_NAME]

# Uncordon a node
kubectl uncordon [NODE_NAME]

# Drain a node (evict pods)
kubectl drain [NODE_NAME] --ignore-daemonsets

# Get node resource usage (requires metrics-server)
kubectl top nodes

# Get node with specific labels
kubectl get nodes -l [KEY]=[VALUE]
```

---

## Namespaces

### Namespace Management
```bash
# List all namespaces
kubectl get namespaces

# Create a namespace
kubectl create namespace [NAMESPACE_NAME]

# Delete a namespace
kubectl delete namespace [NAMESPACE_NAME]

# Describe a namespace
kubectl describe namespace [NAMESPACE_NAME]

# Get resources in a namespace
kubectl get all -n [NAMESPACE_NAME]

# Get resources in all namespaces
kubectl get all --all-namespaces

# Set default namespace
kubectl config set-context --current --namespace=[NAMESPACE_NAME]

# Create namespace from YAML
kubectl apply -f namespace.yaml
```

---

## Pods

### Pod Operations
```bash
# List all pods
kubectl get pods

# List pods in a namespace
kubectl get pods -n [NAMESPACE]

# List all pods in all namespaces
kubectl get pods --all-namespaces

# Get pod details
kubectl describe pod [POD_NAME]

# Get pod in YAML format
kubectl get pod [POD_NAME] -o yaml

# Get pod in JSON format
kubectl get pod [POD_NAME] -o json

# Create a pod
kubectl run [POD_NAME] --image=[IMAGE_NAME]

# Create a pod with specific port
kubectl run [POD_NAME] --image=[IMAGE_NAME] --port=[PORT]

# Create a pod with environment variables
kubectl run [POD_NAME] --image=[IMAGE_NAME] --env="KEY=VALUE"

# Delete a pod
kubectl delete pod [POD_NAME]

# Delete pod immediately
kubectl delete pod [POD_NAME] --grace-period=0 --force

# Execute command in a pod
kubectl exec [POD_NAME] -- [COMMAND]

# Execute interactive shell in a pod
kubectl exec -it [POD_NAME] -- /bin/bash

# Execute command in specific container
kubectl exec [POD_NAME] -c [CONTAINER_NAME] -- [COMMAND]

# Copy files to/from pod
kubectl cp [POD_NAME]:[PATH] [LOCAL_PATH]
kubectl cp [LOCAL_PATH] [POD_NAME]:[PATH]

# Port forward to a pod
kubectl port-forward [POD_NAME] [LOCAL_PORT]:[POD_PORT]

# Get pod logs
kubectl logs [POD_NAME]

# Get pod logs for specific container
kubectl logs [POD_NAME] -c [CONTAINER_NAME]

# Follow pod logs (stream)
kubectl logs -f [POD_NAME]

# Get previous pod logs
kubectl logs [POD_NAME] --previous

# Get pod resource usage
kubectl top pod [POD_NAME]

# Get pods with labels
kubectl get pods -l [KEY]=[VALUE]

# Get pods with wide output (shows node)
kubectl get pods -o wide

# Watch pods
kubectl get pods --watch
```

### Pod Creation from YAML
```bash
# Create/update pod from file
kubectl apply -f pod.yaml

# Create pod from file
kubectl create -f pod.yaml

# Delete pod using file
kubectl delete -f pod.yaml

# Create from URL
kubectl apply -f https://example.com/pod.yaml
```

---

## Deployments

### Deployment Management
```bash
# List deployments
kubectl get deployments

# Describe deployment
kubectl describe deployment [DEPLOYMENT_NAME]

# Create deployment
kubectl create deployment [DEPLOYMENT_NAME] --image=[IMAGE_NAME]

# Create deployment with replicas
kubectl create deployment [DEPLOYMENT_NAME] --image=[IMAGE_NAME] --replicas=[NUM]

# Delete deployment
kubectl delete deployment [DEPLOYMENT_NAME]

# Scale deployment
kubectl scale deployment [DEPLOYMENT_NAME] --replicas=[NUM]

# Autoscale deployment
kubectl autoscale deployment [DEPLOYMENT_NAME] --min=[MIN] --max=[MAX] --cpu-percent=[PERCENT]

# Update deployment image
kubectl set image deployment/[DEPLOYMENT_NAME] [CONTAINER_NAME]=[NEW_IMAGE]

# Edit deployment
kubectl edit deployment [DEPLOYMENT_NAME]

# Rollout status
kubectl rollout status deployment/[DEPLOYMENT_NAME]

# Rollout history
kubectl rollout history deployment/[DEPLOYMENT_NAME]

# Rollback deployment
kubectl rollout undo deployment/[DEPLOYMENT_NAME]

# Rollback to specific revision
kubectl rollout undo deployment/[DEPLOYMENT_NAME] --to-revision=[REVISION]

# Pause rollout
kubectl rollout pause deployment/[DEPLOYMENT_NAME]

# Resume rollout
kubectl rollout resume deployment/[DEPLOYMENT_NAME]

# Restart deployment
kubectl rollout restart deployment/[DEPLOYMENT_NAME]

# Get deployment in YAML
kubectl get deployment [DEPLOYMENT_NAME] -o yaml

# Create deployment from YAML
kubectl apply -f deployment.yaml
```

---

## Services

### Service Management
```bash
# List services
kubectl get services

# Describe service
kubectl describe service [SERVICE_NAME]

# Create ClusterIP service
kubectl create service clusterip [SERVICE_NAME] --tcp=[PORT]:[TARGET_PORT]

# Create NodePort service
kubectl create service nodeport [SERVICE_NAME] --tcp=[PORT]:[TARGET_PORT]

# Create LoadBalancer service
kubectl create service loadbalancer [SERVICE_NAME] --tcp=[PORT]:[TARGET_PORT]

# Expose deployment as service
kubectl expose deployment [DEPLOYMENT_NAME] --port=[PORT] --target-port=[TARGET_PORT]

# Expose deployment as NodePort
kubectl expose deployment [DEPLOYMENT_NAME] --type=NodePort --port=[PORT]

# Expose deployment as LoadBalancer
kubectl expose deployment [DEPLOYMENT_NAME] --type=LoadBalancer --port=[PORT]

# Delete service
kubectl delete service [SERVICE_NAME]

# Get service endpoints
kubectl get endpoints [SERVICE_NAME]

# Get service in YAML
kubectl get service [SERVICE_NAME] -o yaml

# Create service from YAML
kubectl apply -f service.yaml

# Port forward to service
kubectl port-forward service/[SERVICE_NAME] [LOCAL_PORT]:[SERVICE_PORT]
```

---

## ConfigMaps & Secrets

### ConfigMaps
```bash
# List configmaps
kubectl get configmaps

# Describe configmap
kubectl describe configmap [CONFIGMAP_NAME]

# Create configmap from literal
kubectl create configmap [CONFIGMAP_NAME] --from-literal=[KEY]=[VALUE]

# Create configmap from file
kubectl create configmap [CONFIGMAP_NAME] --from-file=[FILE_PATH]

# Create configmap from directory
kubectl create configmap [CONFIGMAP_NAME] --from-file=[DIR_PATH]

# Create configmap from env file
kubectl create configmap [CONFIGMAP_NAME] --from-env-file=[FILE_PATH]

# Delete configmap
kubectl delete configmap [CONFIGMAP_NAME]

# Get configmap in YAML
kubectl get configmap [CONFIGMAP_NAME] -o yaml

# Edit configmap
kubectl edit configmap [CONFIGMAP_NAME]

# Create configmap from YAML
kubectl apply -f configmap.yaml
```

### Secrets
```bash
# List secrets
kubectl get secrets

# Describe secret
kubectl describe secret [SECRET_NAME]

# Create generic secret from literal
kubectl create secret generic [SECRET_NAME] --from-literal=[KEY]=[VALUE]

# Create secret from file
kubectl create secret generic [SECRET_NAME] --from-file=[FILE_PATH]

# Create TLS secret
kubectl create secret tls [SECRET_NAME] --cert=[CERT_FILE] --key=[KEY_FILE]

# Create docker registry secret
kubectl create secret docker-registry [SECRET_NAME] \
  --docker-server=[SERVER] \
  --docker-username=[USERNAME] \
  --docker-password=[PASSWORD] \
  --docker-email=[EMAIL]

# Delete secret
kubectl delete secret [SECRET_NAME]

# Get secret in YAML
kubectl get secret [SECRET_NAME] -o yaml

# Decode secret
kubectl get secret [SECRET_NAME] -o jsonpath='{.data.[KEY]}' | base64 -d

# Create secret from YAML
kubectl apply -f secret.yaml
```

---

## Persistent Volumes

### PersistentVolume (PV) Operations
```bash
# List persistent volumes
kubectl get pv

# Describe persistent volume
kubectl describe pv [PV_NAME]

# Delete persistent volume
kubectl delete pv [PV_NAME]

# Get PV in YAML
kubectl get pv [PV_NAME] -o yaml

# Create PV from YAML
kubectl apply -f pv.yaml
```

### PersistentVolumeClaim (PVC) Operations
```bash
# List persistent volume claims
kubectl get pvc

# List PVCs in namespace
kubectl get pvc -n [NAMESPACE]

# Describe PVC
kubectl describe pvc [PVC_NAME]

# Delete PVC
kubectl delete pvc [PVC_NAME]

# Get PVC in YAML
kubectl get pvc [PVC_NAME] -o yaml

# Create PVC from YAML
kubectl apply -f pvc.yaml
```

### StorageClass Operations
```bash
# List storage classes
kubectl get storageclass

# Describe storage class
kubectl describe storageclass [STORAGECLASS_NAME]

# Get storage class in YAML
kubectl get storageclass [STORAGECLASS_NAME] -o yaml

# Create storage class from YAML
kubectl apply -f storageclass.yaml
```

---

## StatefulSets

### StatefulSet Management
```bash
# List statefulsets
kubectl get statefulsets

# Describe statefulset
kubectl describe statefulset [STATEFULSET_NAME]

# Create statefulset from YAML
kubectl apply -f statefulset.yaml

# Delete statefulset
kubectl delete statefulset [STATEFULSET_NAME]

# Scale statefulset
kubectl scale statefulset [STATEFULSET_NAME] --replicas=[NUM]

# Update statefulset image
kubectl set image statefulset/[STATEFULSET_NAME] [CONTAINER_NAME]=[NEW_IMAGE]

# Get statefulset in YAML
kubectl get statefulset [STATEFULSET_NAME] -o yaml

# Rollout status
kubectl rollout status statefulset/[STATEFULSET_NAME]

# Rollout history
kubectl rollout history statefulset/[STATEFULSET_NAME]
```

---

## DaemonSets

### DaemonSet Management
```bash
# List daemonsets
kubectl get daemonsets

# Describe daemonset
kubectl describe daemonset [DAEMONSET_NAME]

# Create daemonset from YAML
kubectl apply -f daemonset.yaml

# Delete daemonset
kubectl delete daemonset [DAEMONSET_NAME]

# Update daemonset image
kubectl set image daemonset/[DAEMONSET_NAME] [CONTAINER_NAME]=[NEW_IMAGE]

# Get daemonset in YAML
kubectl get daemonset [DAEMONSET_NAME] -o yaml

# Rollout status
kubectl rollout status daemonset/[DAEMONSET_NAME]
```

---

## Jobs & CronJobs

### Jobs
```bash
# List jobs
kubectl get jobs

# Describe job
kubectl describe job [JOB_NAME]

# Create job from image
kubectl create job [JOB_NAME] --image=[IMAGE_NAME]

# Create job from cronjob
kubectl create job [JOB_NAME] --from=cronjob/[CRONJOB_NAME]

# Delete job
kubectl delete job [JOB_NAME]

# Get job in YAML
kubectl get job [JOB_NAME] -o yaml

# Create job from YAML
kubectl apply -f job.yaml

# Get job logs
kubectl logs job/[JOB_NAME]
```

### CronJobs
```bash
# List cronjobs
kubectl get cronjobs

# Describe cronjob
kubectl describe cronjob [CRONJOB_NAME]

# Create cronjob
kubectl create cronjob [CRONJOB_NAME] --image=[IMAGE_NAME] --schedule="[CRON_SCHEDULE]"

# Delete cronjob
kubectl delete cronjob [CRONJOB_NAME]

# Suspend cronjob
kubectl patch cronjob [CRONJOB_NAME] -p '{"spec":{"suspend":true}}'

# Resume cronjob
kubectl patch cronjob [CRONJOB_NAME] -p '{"spec":{"suspend":false}}'

# Get cronjob in YAML
kubectl get cronjob [CRONJOB_NAME] -o yaml

# Create cronjob from YAML
kubectl apply -f cronjob.yaml
```

---

## Ingress

### Ingress Management
```bash
# List ingresses
kubectl get ingress

# Describe ingress
kubectl describe ingress [INGRESS_NAME]

# Create ingress from YAML
kubectl apply -f ingress.yaml

# Delete ingress
kubectl delete ingress [INGRESS_NAME]

# Get ingress in YAML
kubectl get ingress [INGRESS_NAME] -o yaml

# Edit ingress
kubectl edit ingress [INGRESS_NAME]

# List ingress classes
kubectl get ingressclass

# Describe ingress class
kubectl describe ingressclass [INGRESSCLASS_NAME]
```

---

## Resource Management

### Resource Quotas
```bash
# List resource quotas
kubectl get resourcequotas

# Describe resource quota
kubectl describe resourcequota [QUOTA_NAME]

# Create resource quota from YAML
kubectl apply -f resourcequota.yaml

# Delete resource quota
kubectl delete resourcequota [QUOTA_NAME]
```

### Limit Ranges
```bash
# List limit ranges
kubectl get limitranges

# Describe limit range
kubectl describe limitrange [LIMITRANGE_NAME]

# Create limit range from YAML
kubectl apply -f limitrange.yaml

# Delete limit range
kubectl delete limitrange [LIMITRANGE_NAME]
```

### Horizontal Pod Autoscaler (HPA)
```bash
# List HPAs
kubectl get hpa

# Describe HPA
kubectl describe hpa [HPA_NAME]

# Create HPA
kubectl autoscale deployment [DEPLOYMENT_NAME] --min=[MIN] --max=[MAX] --cpu-percent=[PERCENT]

# Delete HPA
kubectl delete hpa [HPA_NAME]

# Get HPA in YAML
kubectl get hpa [HPA_NAME] -o yaml
```

### Network Policies
```bash
# List network policies
kubectl get networkpolicies

# Describe network policy
kubectl describe networkpolicy [POLICY_NAME]

# Create network policy from YAML
kubectl apply -f networkpolicy.yaml

# Delete network policy
kubectl delete networkpolicy [POLICY_NAME]

# Get network policy in YAML
kubectl get networkpolicy [POLICY_NAME] -o yaml
```

---

## Logs & Debugging

### Viewing Logs
```bash
# Get pod logs
kubectl logs [POD_NAME]

# Follow logs (stream)
kubectl logs -f [POD_NAME]

# Get logs from specific container
kubectl logs [POD_NAME] -c [CONTAINER_NAME]

# Get previous container logs
kubectl logs [POD_NAME] --previous

# Get logs with timestamp
kubectl logs [POD_NAME] --timestamps

# Get logs since specific time
kubectl logs [POD_NAME] --since=1h

# Get last N lines of logs
kubectl logs [POD_NAME] --tail=100

# Get logs from all containers in pod
kubectl logs [POD_NAME] --all-containers

# Get logs from deployment
kubectl logs deployment/[DEPLOYMENT_NAME]

# Get logs from multiple pods
kubectl logs -l [KEY]=[VALUE]
```

### Debugging
```bash
# Execute shell in pod
kubectl exec -it [POD_NAME] -- /bin/bash

# Run debug container in pod
kubectl debug [POD_NAME] -it --image=[DEBUG_IMAGE]

# Debug node
kubectl debug node/[NODE_NAME] -it --image=[DEBUG_IMAGE]

# Get events
kubectl get events

# Get events sorted by time
kubectl get events --sort-by='.lastTimestamp'

# Watch events
kubectl get events --watch

# Describe resource for debugging
kubectl describe [RESOURCE_TYPE] [RESOURCE_NAME]

# Get resource in YAML for debugging
kubectl get [RESOURCE_TYPE] [RESOURCE_NAME] -o yaml

# Check pod status
kubectl get pod [POD_NAME] -o jsonpath='{.status.phase}'

# Get pod restart count
kubectl get pod [POD_NAME] -o jsonpath='{.status.containerStatuses[0].restartCount}'

# Get pod conditions
kubectl get pod [POD_NAME] -o jsonpath='{.status.conditions[*].type}'
```

---

## Security & RBAC

### Service Accounts
```bash
# List service accounts
kubectl get serviceaccounts

# Describe service account
kubectl describe serviceaccount [SA_NAME]

# Create service account
kubectl create serviceaccount [SA_NAME]

# Delete service account
kubectl delete serviceaccount [SA_NAME]

# Get service account in YAML
kubectl get serviceaccount [SA_NAME] -o yaml
```

### Roles & RoleBindings
```bash
# List roles
kubectl get roles

# Describe role
kubectl describe role [ROLE_NAME]

# Create role
kubectl create role [ROLE_NAME] --verb=[VERBS] --resource=[RESOURCES]

# Delete role
kubectl delete role [ROLE_NAME]

# List role bindings
kubectl get rolebindings

# Describe role binding
kubectl describe rolebinding [ROLEBINDING_NAME]

# Create role binding
kubectl create rolebinding [BINDING_NAME] --role=[ROLE_NAME] --user=[USER_NAME]

# Create role binding for service account
kubectl create rolebinding [BINDING_NAME] --role=[ROLE_NAME] --serviceaccount=[NAMESPACE]:[SA_NAME]

# Delete role binding
kubectl delete rolebinding [ROLEBINDING_NAME]
```

### ClusterRoles & ClusterRoleBindings
```bash
# List cluster roles
kubectl get clusterroles

# Describe cluster role
kubectl describe clusterrole [CLUSTERROLE_NAME]

# Create cluster role
kubectl create clusterrole [CLUSTERROLE_NAME] --verb=[VERBS] --resource=[RESOURCES]

# Delete cluster role
kubectl delete clusterrole [CLUSTERROLE_NAME]

# List cluster role bindings
kubectl get clusterrolebindings

# Describe cluster role binding
kubectl describe clusterrolebinding [BINDING_NAME]

# Create cluster role binding
kubectl create clusterrolebinding [BINDING_NAME] --clusterrole=[CLUSTERROLE_NAME] --user=[USER_NAME]

# Create cluster role binding for service account
kubectl create clusterrolebinding [BINDING_NAME] --clusterrole=[CLUSTERROLE_NAME] --serviceaccount=[NAMESPACE]:[SA_NAME]

# Delete cluster role binding
kubectl delete clusterrolebinding [BINDING_NAME]
```

### Authorization
```bash
# Check if you can perform action
kubectl auth can-i [VERB] [RESOURCE]

# Check if user can perform action
kubectl auth can-i [VERB] [RESOURCE] --as=[USER_NAME]

# Check if service account can perform action
kubectl auth can-i [VERB] [RESOURCE] --as=system:serviceaccount:[NAMESPACE]:[SA_NAME]

# List what you can do
kubectl auth can-i --list

# Reconcile RBAC resources
kubectl auth reconcile -f [RBAC_FILE]
```

### Pod Security
```bash
# Get pod security policies (deprecated in 1.25+)
kubectl get psp

# Describe pod security policy
kubectl describe psp [PSP_NAME]

# Get pod security standards (1.23+)
kubectl label namespace [NAMESPACE] pod-security.kubernetes.io/enforce=[LEVEL]

# Pod security levels: privileged, baseline, restricted
```

---

## Networking

### Network Debugging
```bash
# Run network debug pod
kubectl run netdebug --image=nicolaka/netshoot -it --rm -- /bin/bash

# Test DNS resolution
kubectl run dnstest --image=busybox:1.28 --rm -it -- nslookup [SERVICE_NAME]

# Test network connectivity
kubectl run nettest --image=busybox --rm -it -- wget -O- [URL]

# Get service cluster IP
kubectl get service [SERVICE_NAME] -o jsonpath='{.spec.clusterIP}'

# Get pod IP
kubectl get pod [POD_NAME] -o jsonpath='{.status.podIP}'

# Get node IPs
kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="InternalIP")].address}'
```

---

## Helm

### Helm Repository Management
```bash
# Add Helm repository
helm repo add [REPO_NAME] [REPO_URL]

# Update Helm repositories
helm repo update

# List Helm repositories
helm repo list

# Remove Helm repository
helm repo remove [REPO_NAME]

# Search for charts in repository
helm search repo [KEYWORD]

# Search Helm Hub
helm search hub [KEYWORD]
```

### Helm Release Management
```bash
# Install Helm chart
helm install [RELEASE_NAME] [CHART_NAME]

# Install chart from repository
helm install [RELEASE_NAME] [REPO_NAME]/[CHART_NAME]

# Install chart with custom values
helm install [RELEASE_NAME] [CHART_NAME] -f values.yaml

# Install chart with set values
helm install [RELEASE_NAME] [CHART_NAME] --set [KEY]=[VALUE]

# Install chart in namespace
helm install [RELEASE_NAME] [CHART_NAME] -n [NAMESPACE]

# Install chart with dry run
helm install [RELEASE_NAME] [CHART_NAME] --dry-run --debug

# Upgrade release
helm upgrade [RELEASE_NAME] [CHART_NAME]

# Upgrade with values
helm upgrade [RELEASE_NAME] [CHART_NAME] -f values.yaml

# Upgrade or install (install if not exists)
helm upgrade --install [RELEASE_NAME] [CHART_NAME]

# Rollback release
helm rollback [RELEASE_NAME] [REVISION]

# Uninstall release
helm uninstall [RELEASE_NAME]

# Uninstall and keep history
helm uninstall [RELEASE_NAME] --keep-history

# List releases
helm list

# List releases in all namespaces
helm list --all-namespaces

# List all releases including failed
helm list --all

# Get release status
helm status [RELEASE_NAME]

# Get release history
helm history [RELEASE_NAME]

# Get release values
helm get values [RELEASE_NAME]

# Get release manifest
helm get manifest [RELEASE_NAME]

# Get release notes
helm get notes [RELEASE_NAME]
```

### Helm Chart Management
```bash
# Create new chart
helm create [CHART_NAME]

# Package chart
helm package [CHART_PATH]

# Lint chart
helm lint [CHART_PATH]

# Template chart (render templates)
helm template [RELEASE_NAME] [CHART_PATH]

# Show chart information
helm show chart [CHART_NAME]

# Show chart values
helm show values [CHART_NAME]

# Show chart README
helm show readme [CHART_NAME]

# Show all chart information
helm show all [CHART_NAME]

# Pull chart
helm pull [CHART_NAME]

# Pull and untar chart
helm pull [CHART_NAME] --untar

# Verify chart
helm verify [CHART_PATH]
```

---

## kubectl Tips & Tricks

### Aliases & Shortcuts
```bash
# Set up common aliases
alias k=kubectl
alias kgp='kubectl get pods'
alias kgs='kubectl get services'
alias kgd='kubectl get deployments'
alias kgn='kubectl get nodes'
alias kdp='kubectl describe pod'
alias kds='kubectl describe service'
alias kdd='kubectl describe deployment'
alias kl='kubectl logs'
alias klf='kubectl logs -f'
alias kex='kubectl exec -it'

# Enable kubectl autocompletion (bash)
source <(kubectl completion bash)
alias k=kubectl
complete -F __start_kubectl k

# Enable kubectl autocompletion (zsh)
source <(kubectl completion zsh)
```

### Output Formatting
```bash
# Get resources in JSON
kubectl get [RESOURCE] -o json

# Get resources in YAML
kubectl get [RESOURCE] -o yaml

# Get resources in wide format
kubectl get [RESOURCE] -o wide

# Get resource names only
kubectl get [RESOURCE] -o name

# Custom columns output
kubectl get [RESOURCE] -o custom-columns=[NAME]:[JSONPATH]

# Example: Get pod name and IP
kubectl get pods -o custom-columns=NAME:.metadata.name,IP:.status.podIP

# JSONPath output
kubectl get [RESOURCE] -o jsonpath='{.items[*].[FIELD]}'

# Example: Get all pod IPs
kubectl get pods -o jsonpath='{.items[*].status.podIP}'

# Pretty print JSON
kubectl get [RESOURCE] -o json | jq '.'

# Sort by field
kubectl get [RESOURCE] --sort-by=[JSONPATH]

# Example: Sort pods by creation time
kubectl get pods --sort-by=.metadata.creationTimestamp
```

### Label & Selector Operations
```bash
# Get resources with label
kubectl get [RESOURCE] -l [KEY]=[VALUE]

# Get resources with label selector
kubectl get [RESOURCE] -l '[KEY] in ([VALUE1],[VALUE2])'

# Get resources without label
kubectl get [RESOURCE] -l '[KEY]!=[VALUE]'

# Get resources with label key exists
kubectl get [RESOURCE] -l [KEY]

# Get resources where label key doesn't exist
kubectl get [RESOURCE] -l '![KEY]'

# Add label to resource
kubectl label [RESOURCE] [NAME] [KEY]=[VALUE]

# Remove label from resource
kubectl label [RESOURCE] [NAME] [KEY]-

# Overwrite existing label
kubectl label [RESOURCE] [NAME] [KEY]=[VALUE] --overwrite

# Label multiple resources
kubectl label [RESOURCE] --all [KEY]=[VALUE]
```

### Field Selectors
```bash
# Get pods by field selector
kubectl get pods --field-selector status.phase=Running

# Get services by field selector
kubectl get services --field-selector metadata.namespace=default

# Combine multiple field selectors
kubectl get pods --field-selector status.phase=Running,spec.restartPolicy=Always
```

### Resource Management
```bash
# Apply configuration from file or directory
kubectl apply -f [FILE_OR_DIR]

# Apply configuration recursively
kubectl apply -f [DIR] -R

# Apply and record change cause
kubectl apply -f [FILE] --record

# Apply with server-side apply
kubectl apply -f [FILE] --server-side

# Create resource from file
kubectl create -f [FILE]

# Replace resource from file
kubectl replace -f [FILE]

# Force replace resource
kubectl replace -f [FILE] --force

# Delete resource from file
kubectl delete -f [FILE]

# Diff before applying
kubectl diff -f [FILE]

# Patch resource
kubectl patch [RESOURCE] [NAME] -p '[PATCH_JSON]'

# Example: Patch deployment image
kubectl patch deployment [NAME] -p '{"spec":{"template":{"spec":{"containers":[{"name":"[CONTAINER]","image":"[IMAGE]"}]}}}}'
```

### Context & Namespace Management
```bash
# Get all contexts
kubectl config get-contexts

# Get current context
kubectl config current-context

# Use specific context
kubectl config use-context [CONTEXT_NAME]

# Set default namespace for context
kubectl config set-context --current --namespace=[NAMESPACE]

# Create new context
kubectl config set-context [CONTEXT_NAME] --cluster=[CLUSTER] --user=[USER] --namespace=[NAMESPACE]

# Delete context
kubectl config delete-context [CONTEXT_NAME]

# Get current namespace
kubectl config view --minify --output 'jsonpath={..namespace}'
```

### Quick Actions
```bash
# Run temporary pod
kubectl run tmp-shell --rm -i --tty --image=busybox -- /bin/sh

# Run curl test
kubectl run curl-test --rm -i --tty --image=curlimages/curl -- sh

# Create YAML from kubectl command
kubectl create deployment [NAME] --image=[IMAGE] --dry-run=client -o yaml > deployment.yaml

# Generate secret YAML
kubectl create secret generic [NAME] --from-literal=[KEY]=[VALUE] --dry-run=client -o yaml > secret.yaml

# Watch resource changes
kubectl get [RESOURCE] --watch

# Wait for condition
kubectl wait --for=condition=Ready pod/[POD_NAME]

# Wait for deletion
kubectl wait --for=delete pod/[POD_NAME] --timeout=60s

# Get all resources in namespace
kubectl get all -n [NAMESPACE]

# Delete all resources in namespace
kubectl delete all --all -n [NAMESPACE]

# Force delete pod
kubectl delete pod [POD_NAME] --grace-period=0 --force

# Explain resource fields
kubectl explain [RESOURCE].[FIELD]
```

### Performance & Monitoring
```bash
# Get resource usage (requires metrics-server)
kubectl top nodes

# Get pod resource usage
kubectl top pods

# Get pod resource usage in namespace
kubectl top pods -n [NAMESPACE]

# Get pod resource usage with containers
kubectl top pods --containers

# Get pod resource usage sorted by memory
kubectl top pods --sort-by=memory

# Get pod resource usage sorted by CPU
kubectl top pods --sort-by=cpu
```

---

## Common Patterns & Best Practices

### Deployment Strategies
```bash
# Rolling update (default)
kubectl set image deployment/[NAME] [CONTAINER]=[IMAGE]

# Check rollout status
kubectl rollout status deployment/[NAME]

# Pause rollout
kubectl rollout pause deployment/[NAME]

# Resume rollout
kubectl rollout resume deployment/[NAME]

# Undo rollout
kubectl rollout undo deployment/[NAME]

# View rollout history
kubectl rollout history deployment/[NAME]

# Rollback to specific revision
kubectl rollout undo deployment/[NAME] --to-revision=[NUM]
```

### Resource Cleanup
```bash
# Delete completed pods
kubectl delete pods --field-selector=status.phase==Succeeded

# Delete failed pods
kubectl delete pods --field-selector=status.phase==Failed

# Delete evicted pods
kubectl get pods | grep Evicted | awk '{print $1}' | xargs kubectl delete pod

# Delete all resources with label
kubectl delete all -l [KEY]=[VALUE]

# Delete namespace and all resources
kubectl delete namespace [NAMESPACE]
```

### Troubleshooting Commands
```bash
# Get pod events
kubectl get events --field-selector involvedObject.name=[POD_NAME]

# Get recent events
kubectl get events --sort-by='.lastTimestamp' | tail -20

# Check pod logs
kubectl logs [POD_NAME] --previous

# Describe pod for errors
kubectl describe pod [POD_NAME]

# Check node conditions
kubectl describe node [NODE_NAME] | grep Conditions -A 10

# Get pod container status
kubectl get pod [POD_NAME] -o jsonpath='{.status.containerStatuses[*].state}'

# Check resource quotas
kubectl describe quota -n [NAMESPACE]

# Check limit ranges
kubectl describe limitrange -n [NAMESPACE]
```

---

## Quick Reference

### Resource Types (Short Names)
```
pods (po)
services (svc)
deployments (deploy)
replicasets (rs)
statefulsets (sts)
daemonsets (ds)
jobs
cronjobs (cj)
nodes (no)
namespaces (ns)
configmaps (cm)
secrets
persistentvolumes (pv)
persistentvolumeclaims (pvc)
ingress (ing)
networkpolicies (netpol)
serviceaccounts (sa)
```

### Common kubectl Flags
```
-n, --namespace          Specify namespace
-A, --all-namespaces     All namespaces
-l, --selector           Label selector
-o, --output             Output format (json, yaml, wide, name, etc.)
-w, --watch              Watch for changes
--dry-run=client         Dry run (client-side)
--dry-run=server         Dry run (server-side)
-f, --filename           File or directory
-R, --recursive          Process directory recursively
--force                  Force operation
--grace-period           Seconds before force delete
-h, --help               Help for command
```

### Useful Tips
1. Use `kubectl explain` to understand resource fields
2. Use `--dry-run=client -o yaml` to generate YAML templates
3. Use labels and selectors for efficient resource management
4. Always specify namespace explicitly for production operations
5. Use contexts to manage multiple clusters
6. Set up kubectl autocompletion for faster typing
7. Use `kubectl diff` before applying changes
8. Keep YAML files in version control
9. Use namespace quotas and limit ranges for resource management
10. Regularly check events and logs for troubleshooting

---

**Note**: Replace placeholders like `[POD_NAME]`, `[DEPLOYMENT_NAME]`, etc., with actual resource names when using these commands.

For more information, visit the [official Kubernetes documentation](https://kubernetes.io/docs/).
