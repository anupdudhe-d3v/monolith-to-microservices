### step by step procedure to deploy microservices project in gcp gke 

### create a vpc with appropriate subnets and secondary and primary ip for pods and services
# if you want complete range to be occupied then use below cli
``` 
# Create VPC
gcloud compute networks create gke-vpc --subnet-mode=custom

# Create subnet with secondary ranges
gcloud compute networks subnets create gke-subnet \
  --network=gke-vpc \
  --range=10.0.0.0/16 \
  --region=us-west1 \
  --secondary-range=pods-range=10.1.0.0/17,services-range=10.2.0.0/20

# Create GKE cluster
gcloud container clusters create-auto my-gke-cluster \
  --region=us-west1 \
  --network=gke-vpc \
  --subnetwork=gke-subnet \
  --cluster-secondary-range-name=pods-range \
  --services-secondary-range-name=services-range
```
# if limited number of ips are desired use below cli
```
gcloud compute networks create gke-vpc --subnet-mode=custom

gcloud compute networks subnets create gke-subnet \
  --network=gke-vpc \
  --range=10.0.0.0/21 \
  --region=us-west1 \
  --secondary-range=pods-range=10.1.0.0/22,services-range=10.2.0.0/24

gcloud container clusters create-auto my-gke-cluster \
  --region=us-west1 \
  --network=gke-vpc \
  --subnetwork=gke-subnet \
  --cluster-secondary-range-name=pods-range \
  --services-secondary-range-name=services-range
``` 
### pushing docker images to registry

```
gcloud services enable containerregistry.googleapis.com
gcloud auth configure-docker
docker tag dockerimgid gcr.io/projectid/frontend:v1
gcloud auth configure-docker
docker push gcr.io/projectid/frontend:v1
```
```
docker tag imageid frontend
```
### once cluster is created hit following command you get this command from gke as well
```
gcloud container clusters get-credentials my-gke-cluster --region us-central1 --project qwiklabs-gcp-02-d4016954c983
```