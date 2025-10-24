# GCloud Commands Cheat Sheet for GCP ACE Certification

## Table of Contents
- [Authentication & Configuration](#authentication--configuration)
- [Compute Engine (GCE)](#compute-engine-gce)
- [Cloud Storage (GCS)](#cloud-storage-gcs)
- [Identity & Access Management (IAM)](#identity--access-management-iam)
- [Google Kubernetes Engine (GKE)](#google-kubernetes-engine-gke)
- [App Engine](#app-engine)
- [Cloud SQL](#cloud-sql)
- [Networking](#networking)
- [Billing & Projects](#billing--projects)
- [Monitoring & Logging](#monitoring--logging)

---

## Authentication & Configuration

### Initialize gcloud
```bash
# Initialize gcloud and authenticate
gcloud init

# Login to gcloud
gcloud auth login

# Set application default credentials
gcloud auth application-default login

# Revoke credentials
gcloud auth revoke
```

### Configuration Management
```bash
# List configurations
gcloud config configurations list

# Create a new configuration
gcloud config configurations create [CONFIG_NAME]

# Activate a configuration
gcloud config configurations activate [CONFIG_NAME]

# Set project
gcloud config set project [PROJECT_ID]

# Set compute zone
gcloud config set compute/zone [ZONE]

# Set compute region
gcloud config set compute/region [REGION]

# Get current configuration
gcloud config list

# Unset a property
gcloud config unset [PROPERTY]
```

---

## Compute Engine (GCE)

### Instance Management
```bash
# List all instances
gcloud compute instances list

# Create an instance
gcloud compute instances create [INSTANCE_NAME] \
  --machine-type=[MACHINE_TYPE] \
  --zone=[ZONE] \
  --image-family=[IMAGE_FAMILY] \
  --image-project=[IMAGE_PROJECT]

# Create an instance with startup script
gcloud compute instances create [INSTANCE_NAME] \
  --metadata-from-file startup-script=[SCRIPT_PATH]

# Start an instance
gcloud compute instances start [INSTANCE_NAME] --zone=[ZONE]

# Stop an instance
gcloud compute instances stop [INSTANCE_NAME] --zone=[ZONE]

# Delete an instance
gcloud compute instances delete [INSTANCE_NAME] --zone=[ZONE]

# SSH into an instance
gcloud compute ssh [INSTANCE_NAME] --zone=[ZONE]

# Describe instance details
gcloud compute instances describe [INSTANCE_NAME] --zone=[ZONE]

# Reset an instance
gcloud compute instances reset [INSTANCE_NAME] --zone=[ZONE]

# Attach a disk
gcloud compute instances attach-disk [INSTANCE_NAME] \
  --disk=[DISK_NAME] --zone=[ZONE]

# Detach a disk
gcloud compute instances detach-disk [INSTANCE_NAME] \
  --disk=[DISK_NAME] --zone=[ZONE]
```

### Disks
```bash
# List disks
gcloud compute disks list

# Create a disk
gcloud compute disks create [DISK_NAME] \
  --size=[SIZE] --zone=[ZONE] --type=[DISK_TYPE]

# Delete a disk
gcloud compute disks delete [DISK_NAME] --zone=[ZONE]

# Describe a disk
gcloud compute disks describe [DISK_NAME] --zone=[ZONE]

# Create a snapshot
gcloud compute disks snapshot [DISK_NAME] \
  --snapshot-names=[SNAPSHOT_NAME] --zone=[ZONE]

# List snapshots
gcloud compute snapshots list

# Delete a snapshot
gcloud compute snapshots delete [SNAPSHOT_NAME]
```

### Images
```bash
# List images
gcloud compute images list

# Create an image from a disk
gcloud compute images create [IMAGE_NAME] \
  --source-disk=[DISK_NAME] --source-disk-zone=[ZONE]

# Delete an image
gcloud compute images delete [IMAGE_NAME]

# Describe an image
gcloud compute images describe [IMAGE_NAME]
```

### Instance Templates
```bash
# List instance templates
gcloud compute instance-templates list

# Create an instance template
gcloud compute instance-templates create [TEMPLATE_NAME] \
  --machine-type=[MACHINE_TYPE] \
  --image-family=[IMAGE_FAMILY]

# Delete an instance template
gcloud compute instance-templates delete [TEMPLATE_NAME]

# Describe a template
gcloud compute instance-templates describe [TEMPLATE_NAME]
```

### Instance Groups
```bash
# List managed instance groups
gcloud compute instance-groups managed list

# Create a managed instance group
gcloud compute instance-groups managed create [GROUP_NAME] \
  --base-instance-name=[BASE_NAME] \
  --template=[TEMPLATE_NAME] \
  --size=[SIZE] --zone=[ZONE]

# Set autoscaling
gcloud compute instance-groups managed set-autoscaling [GROUP_NAME] \
  --max-num-replicas=[MAX] \
  --min-num-replicas=[MIN] \
  --target-cpu-utilization=[UTILIZATION] \
  --zone=[ZONE]

# Update instances in a group
gcloud compute instance-groups managed rolling-action start-update [GROUP_NAME] \
  --version=template=[TEMPLATE_NAME] --zone=[ZONE]

# Delete a managed instance group
gcloud compute instance-groups managed delete [GROUP_NAME] --zone=[ZONE]
```

---

## Cloud Storage (GCS)

### Bucket Management
```bash
# List all buckets
gsutil ls

# Create a bucket
gsutil mb gs://[BUCKET_NAME]

# Create a bucket with specific location
gsutil mb -l [LOCATION] gs://[BUCKET_NAME]

# Delete a bucket
gsutil rb gs://[BUCKET_NAME]

# Remove bucket and all contents
gsutil rm -r gs://[BUCKET_NAME]
```

### File Operations
```bash
# List contents of a bucket
gsutil ls gs://[BUCKET_NAME]

# Copy a file to bucket
gsutil cp [LOCAL_FILE] gs://[BUCKET_NAME]/

# Copy recursively
gsutil cp -r [LOCAL_DIR] gs://[BUCKET_NAME]/

# Copy from bucket to local
gsutil cp gs://[BUCKET_NAME]/[FILE] [LOCAL_PATH]

# Move a file
gsutil mv gs://[BUCKET_NAME]/[SOURCE] gs://[BUCKET_NAME]/[DEST]

# Delete a file
gsutil rm gs://[BUCKET_NAME]/[FILE]

# Sync directories
gsutil rsync -r [LOCAL_DIR] gs://[BUCKET_NAME]/

# Set object metadata
gsutil setmeta -h "Content-Type:[TYPE]" gs://[BUCKET_NAME]/[FILE]
```

### Permissions & Access
```bash
# Make a file public
gsutil acl ch -u AllUsers:R gs://[BUCKET_NAME]/[FILE]

# Set bucket IAM policy
gsutil iam ch [MEMBER]:[ROLE] gs://[BUCKET_NAME]

# Get bucket IAM policy
gsutil iam get gs://[BUCKET_NAME]

# Set bucket lifecycle
gsutil lifecycle set [LIFECYCLE_JSON] gs://[BUCKET_NAME]

# Enable versioning
gsutil versioning set on gs://[BUCKET_NAME]

# Set uniform bucket-level access
gsutil uniformbucketlevelaccess set on gs://[BUCKET_NAME]
```

---

## Identity & Access Management (IAM)

### Service Accounts
```bash
# List service accounts
gcloud iam service-accounts list

# Create a service account
gcloud iam service-accounts create [SA_NAME] \
  --display-name="[DISPLAY_NAME]"

# Delete a service account
gcloud iam service-accounts delete [SA_EMAIL]

# List keys for a service account
gcloud iam service-accounts keys list \
  --iam-account=[SA_EMAIL]

# Create a key for service account
gcloud iam service-accounts keys create [KEY_FILE] \
  --iam-account=[SA_EMAIL]

# Delete a service account key
gcloud iam service-accounts keys delete [KEY_ID] \
  --iam-account=[SA_EMAIL]
```

### IAM Policies
```bash
# Get IAM policy for a project
gcloud projects get-iam-policy [PROJECT_ID]

# Add IAM policy binding
gcloud projects add-iam-policy-binding [PROJECT_ID] \
  --member=[MEMBER] \
  --role=[ROLE]

# Remove IAM policy binding
gcloud projects remove-iam-policy-binding [PROJECT_ID] \
  --member=[MEMBER] \
  --role=[ROLE]

# List roles
gcloud iam roles list

# Describe a role
gcloud iam roles describe [ROLE_NAME]

# Create a custom role
gcloud iam roles create [ROLE_ID] \
  --project=[PROJECT_ID] \
  --title="[TITLE]" \
  --permissions=[PERMISSIONS]
```

---

## Google Kubernetes Engine (GKE)

### Cluster Management
```bash
# List clusters
gcloud container clusters list

# Create a cluster
gcloud container clusters create [CLUSTER_NAME] \
  --num-nodes=[NUM] --zone=[ZONE]

# Create a cluster with specific machine type
gcloud container clusters create [CLUSTER_NAME] \
  --machine-type=[MACHINE_TYPE] \
  --num-nodes=[NUM] --zone=[ZONE]

# Get cluster credentials
gcloud container clusters get-credentials [CLUSTER_NAME] \
  --zone=[ZONE]

# Resize a cluster
gcloud container clusters resize [CLUSTER_NAME] \
  --num-nodes=[NUM] --zone=[ZONE]

# Upgrade a cluster
gcloud container clusters upgrade [CLUSTER_NAME] \
  --zone=[ZONE]

# Delete a cluster
gcloud container clusters delete [CLUSTER_NAME] \
  --zone=[ZONE]

# Describe a cluster
gcloud container clusters describe [CLUSTER_NAME] \
  --zone=[ZONE]
```

### Node Pools
```bash
# List node pools
gcloud container node-pools list --cluster=[CLUSTER_NAME]

# Create a node pool
gcloud container node-pools create [POOL_NAME] \
  --cluster=[CLUSTER_NAME] \
  --num-nodes=[NUM] --zone=[ZONE]

# Delete a node pool
gcloud container node-pools delete [POOL_NAME] \
  --cluster=[CLUSTER_NAME] --zone=[ZONE]
```

---

## App Engine

### App Management
```bash
# Deploy an application
gcloud app deploy

# Deploy with specific version
gcloud app deploy --version=[VERSION]

# Browse the deployed app
gcloud app browse

# Describe app
gcloud app describe

# List versions
gcloud app versions list

# Stop a version
gcloud app versions stop [VERSION]

# Delete a version
gcloud app versions delete [VERSION]

# View logs
gcloud app logs tail

# Set default service
gcloud app services set-traffic [SERVICE] \
  --splits=[VERSION]=[SPLIT]
```

---

## Cloud SQL

### Instance Management
```bash
# List instances
gcloud sql instances list

# List available database versions
gcloud sql database-versions list

# Create a MySQL instance (use MYSQL_8_0, MYSQL_8_4, etc.)
gcloud sql instances create [INSTANCE_NAME] \
  --database-version=MYSQL_8_0 \
  --tier=[TIER] --region=[REGION]

# Create a PostgreSQL instance (use POSTGRES_14, POSTGRES_13, etc.)
gcloud sql instances create [INSTANCE_NAME] \
  --database-version=POSTGRES_14 \
  --tier=[TIER] --region=[REGION]

# Describe an instance
gcloud sql instances describe [INSTANCE_NAME]

# Delete an instance
gcloud sql instances delete [INSTANCE_NAME]

# Patch (update) an instance
gcloud sql instances patch [INSTANCE_NAME] \
  --tier=[NEW_TIER]

# Restart an instance
gcloud sql instances restart [INSTANCE_NAME]
```

### Database Operations
```bash
# List databases
gcloud sql databases list --instance=[INSTANCE_NAME]

# Create a database
gcloud sql databases create [DATABASE_NAME] \
  --instance=[INSTANCE_NAME]

# Delete a database
gcloud sql databases delete [DATABASE_NAME] \
  --instance=[INSTANCE_NAME]

# Connect to an instance
gcloud sql connect [INSTANCE_NAME] --user=root
```

### Backup & Export
```bash
# List backups
gcloud sql backups list --instance=[INSTANCE_NAME]

# Create a backup
gcloud sql backups create --instance=[INSTANCE_NAME]

# Export data
gcloud sql export sql [INSTANCE_NAME] \
  gs://[BUCKET_NAME]/[FILE] \
  --database=[DATABASE_NAME]

# Import data
gcloud sql import sql [INSTANCE_NAME] \
  gs://[BUCKET_NAME]/[FILE] \
  --database=[DATABASE_NAME]
```

---

## Networking

### VPC Networks
```bash
# List networks
gcloud compute networks list

# Create a network (auto mode)
gcloud compute networks create [NETWORK_NAME] \
  --subnet-mode=auto

# Create a network (custom mode)
gcloud compute networks create [NETWORK_NAME] \
  --subnet-mode=custom

# Delete a network
gcloud compute networks delete [NETWORK_NAME]

# Describe a network
gcloud compute networks describe [NETWORK_NAME]
```

### Subnets
```bash
# List subnets
gcloud compute networks subnets list

# Create a subnet
gcloud compute networks subnets create [SUBNET_NAME] \
  --network=[NETWORK_NAME] \
  --range=[IP_RANGE] --region=[REGION]

# Delete a subnet
gcloud compute networks subnets delete [SUBNET_NAME] \
  --region=[REGION]

# Expand subnet range
gcloud compute networks subnets expand-ip-range [SUBNET_NAME] \
  --prefix-length=[PREFIX_LENGTH] --region=[REGION]
```

### Firewall Rules
```bash
# List firewall rules
gcloud compute firewall-rules list

# Create a firewall rule
gcloud compute firewall-rules create [RULE_NAME] \
  --network=[NETWORK_NAME] \
  --allow=[PROTOCOL]:[PORT] \
  --source-ranges=[IP_RANGES]

# Delete a firewall rule
gcloud compute firewall-rules delete [RULE_NAME]

# Update a firewall rule
gcloud compute firewall-rules update [RULE_NAME] \
  --allow=[PROTOCOL]:[PORT]

# Describe a firewall rule
gcloud compute firewall-rules describe [RULE_NAME]
```

### Load Balancers
```bash
# Create a backend service
gcloud compute backend-services create [BACKEND_NAME] \
  --protocol=HTTP --global

# Add backend to service
gcloud compute backend-services add-backend [BACKEND_NAME] \
  --instance-group=[GROUP_NAME] \
  --instance-group-zone=[ZONE] --global

# Create URL map
gcloud compute url-maps create [URL_MAP_NAME] \
  --default-service=[BACKEND_NAME]

# Create HTTP proxy
gcloud compute target-http-proxies create [PROXY_NAME] \
  --url-map=[URL_MAP_NAME]

# Create forwarding rule
gcloud compute forwarding-rules create [RULE_NAME] \
  --global --target-http-proxy=[PROXY_NAME] --ports=80
```

### VPN
```bash
# Create VPN gateway
gcloud compute target-vpn-gateways create [GATEWAY_NAME] \
  --network=[NETWORK_NAME] --region=[REGION]

# Create VPN tunnel
gcloud compute vpn-tunnels create [TUNNEL_NAME] \
  --peer-address=[PEER_IP] \
  --target-vpn-gateway=[GATEWAY_NAME] \
  --shared-secret=[SECRET] --region=[REGION]
```

---

## Billing & Projects

### Project Management
```bash
# List projects
gcloud projects list

# Create a project
gcloud projects create [PROJECT_ID] \
  --name="[PROJECT_NAME]"

# Delete a project
gcloud projects delete [PROJECT_ID]

# Describe a project
gcloud projects describe [PROJECT_ID]

# Set project property
gcloud config set project [PROJECT_ID]

# Get current project
gcloud config get-value project
```

### Billing
```bash
# List billing accounts (beta - check for GA status)
gcloud beta billing accounts list

# Link project to billing account (beta - check for GA status)
gcloud beta billing projects link [PROJECT_ID] \
  --billing-account=[ACCOUNT_ID]

# Unlink project from billing (beta - check for GA status)
gcloud beta billing projects unlink [PROJECT_ID]

# Get project billing info (beta - check for GA status)
gcloud beta billing projects describe [PROJECT_ID]

# Note: Billing commands are in beta. Check if promoted to GA with:
# gcloud billing --help
```

---

## Monitoring & Logging

### Cloud Logging
```bash
# Read logs
gcloud logging read "[FILTER]" --limit=[NUM]

# Read logs with timestamp filter
gcloud logging read "[FILTER]" \
  --limit=[NUM] \
  --format=json \
  --freshness=[DURATION]

# Write a log entry
gcloud logging write [LOG_NAME] "[MESSAGE]" \
  --severity=[SEVERITY]

# List logs
gcloud logging logs list

# Delete logs
gcloud logging logs delete [LOG_NAME]

# Create a sink
gcloud logging sinks create [SINK_NAME] \
  [DESTINATION] --log-filter="[FILTER]"

# List sinks
gcloud logging sinks list
```

### Monitoring
```bash
# List metrics
gcloud monitoring metrics-descriptors list

# List time series
gcloud monitoring time-series list

# Create an alert policy (alpha - experimental, may change)
gcloud alpha monitoring policies create --policy-from-file=[FILE]

# List alert policies (alpha - experimental, may change)
gcloud alpha monitoring policies list

# Note: Alert policy commands are in alpha. For production use, consider:
# - Using Cloud Console for alert policy management
# - Using Terraform or other IaC tools
# - Checking if promoted to beta/GA with: gcloud monitoring --help
```

---

## Useful General Commands

### Help & Information
```bash
# Get help for any command
gcloud help [COMMAND]

# Get command-specific help
gcloud [SERVICE] [COMMAND] --help

# List available zones
gcloud compute zones list

# List available regions
gcloud compute regions list

# List available machine types
gcloud compute machine-types list --zone=[ZONE]

# List available images
gcloud compute images list

# Get info about current configuration
gcloud info

# Get version
gcloud version

# Update gcloud components
gcloud components update

# Install a component
gcloud components install [COMPONENT]
```

### Filters & Formatting
```bash
# Filter results
gcloud compute instances list --filter="zone:[ZONE]"

# Format output as JSON
gcloud compute instances list --format=json

# Format output as YAML
gcloud compute instances list --format=yaml

# Format output as table
gcloud compute instances list --format="table(name,zone,status)"

# Get specific values
gcloud compute instances list --format="value(name)"

# Limit results
gcloud compute instances list --limit=10

# Sort results
gcloud compute instances list --sort-by=creationTimestamp
```

---

## Memory Tips for GCP ACE Exam

### Command Structure Pattern
Most gcloud commands follow this structure:
```
gcloud [SERVICE] [RESOURCE] [ACTION] [NAME] [FLAGS]
```

**Examples:**
- `gcloud compute instances create my-vm --zone=us-central1-a`
- `gcloud container clusters delete my-cluster --zone=us-central1-a`
- `gcloud projects add-iam-policy-binding my-project --member=user:email@example.com --role=roles/viewer`

### Common Flags to Remember
- `--project=[PROJECT_ID]` - Specify project
- `--zone=[ZONE]` - Specify zone
- `--region=[REGION]` - Specify region
- `--format=[FORMAT]` - Output format (json, yaml, table)
- `--filter="[EXPRESSION]"` - Filter results
- `--quiet` or `-q` - Suppress prompts

### Service Shortcuts
- **GCE**: `gcloud compute`
- **GCS**: `gsutil` (separate tool)
- **GKE**: `gcloud container`
- **IAM**: `gcloud iam` or `gcloud projects [add/remove]-iam-policy-binding`
- **App Engine**: `gcloud app`
- **Cloud SQL**: `gcloud sql`

### Zone vs Region
- **Zone**: Specific data center (e.g., us-central1-a)
- **Region**: Group of zones (e.g., us-central1)
- Some resources are zonal (instances), others are regional (subnets), some are global (images, VPCs)

### Quick Reference for Resource Scope
- **Global**: Images, Instance Templates, VPC Networks, Firewall Rules, URLs Maps
- **Regional**: Subnets, Regional Managed Instance Groups, Cloud SQL
- **Zonal**: VM Instances, Persistent Disks, Zonal Managed Instance Groups

---

## Additional Tips

1. **Practice with `--dry-run`**: Many commands support dry run to see what would happen
2. **Use Tab Completion**: Enable tab completion for faster typing
3. **Check Help**: Use `--help` flag extensively during practice
4. **Format Output**: Practice with different output formats (json, yaml, table)
5. **Use Filters**: Master filtering to quickly find resources
6. **Understand IAM**: Know the difference between primitive, predefined, and custom roles
7. **Know the Hierarchy**: Organization → Folder → Project → Resources
8. **Billing Alerts**: Understand how to set up billing alerts and budgets
9. **Network Concepts**: Master VPC, Subnets, Routes, and Firewall Rules
10. **Practice Labs**: Use GCP Free Tier and Qwiklabs for hands-on practice

---

## Quick Quiz Self-Test

Test yourself with these common scenarios:

1. **Create a VM with a startup script**
2. **Copy files to Cloud Storage and make them public**
3. **Create a GKE cluster and get credentials**
4. **Add a user to a project with viewer role**
5. **Create a load balancer with backend service**
6. **Set up Cloud SQL instance and create a database**
7. **Create a managed instance group with autoscaling**
8. **Configure firewall rule for HTTP traffic**
9. **Create a service account and generate keys**
10. **Export and import Cloud SQL data**

Review this cheat sheet regularly and practice the commands hands-on for best retention!
