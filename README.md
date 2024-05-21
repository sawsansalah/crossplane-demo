 ==================
### Install K’8 Cluster 
'''sh
gcloud container clusters create \
  --machine-type n1-standard-2 \
  --num-nodes 2 \
  --zone us-central1-c \
  --no-enable-ip-alias \
  --cluster-version latest \
  crossplane-demo

'''sh

### Install Crossplane

```sh
helm repo add \
crossplane-stable https://charts.crossplane.io/stable && helm repo update

helm install crossplane \
--namespace crossplane-system \
--create-namespace crossplane-stable/crossplane
```
Verify Installation
```sh
kubectl get pods -n crossplane-system
```
### Install GCP Provider

```sh
kubectl apply -f provider.yaml
```
Verify Installation
```sh
kubectl get providers
```

### Create a Service Account Key

Service Account should have permissions to deploy the resources. In this example we will deploy GCS, hence it needs storage admin permissions

###  Create Secret from that Service account key

```sh
kubectl create secret \
> generic gcp-secret \
> -n crossplane-system \
> --from-file=creds=./gcp-credentials.json
```
###  Create Secret from that Service account key

```sh
kubectl apply -f providerConfig.yaml
```

Verify Installation

```sh
kubectl get providerconfig
```
### create bucket 

```sh
kubectl apply -f bucket.yaml
```
Verify Installation

```sh
 kubectl get bucket
```
