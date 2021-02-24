# Google

## KMS

Create crypto key together with service account

```bash
#!/usr/bin/env bash
set -uex

projectId="myProject"
cluster="myCluster"
keyRing="$cluster"
keyName="$cluster-vault"
# https://cloud.google.com/kms/docs/locations
location='europe-west4'
saKeyFile="$cluster-key.json"

function create_keyring() {
  gcloud kms keyrings create $keyRing \
    --location $location
}

function create_kms_key() {
  gcloud kms keys create $keyName \
    --keyring $keyRing \
    --location $location \
    --purpose "encryption"
}

function create_sa() {
  # https://cloud.google.com/iam/docs/creating-managing-service-accounts#iam-service-accounts-create-gcloud
  gcloud iam service-accounts create $cluster \
    --description="Manage resources for $cluster cluster" \
    --display-name="$cluster"
}

function add_role() {
  # https://cloud.google.com/iam/docs/understanding-roles#cloud-kms-roles
  gcloud projects add-iam-policy-binding "$projectId" \
    --member="serviceAccount:${cluster}@${projectId}.iam.gserviceaccount.com" \
    --role="roles/cloudkms.cryptoKeyEncrypterDecrypter"
}

function generate_sa_key() {
  # https://cloud.google.com/iam/docs/creating-managing-service-account-keys
  gcloud iam service-accounts keys create ${saKeyFile} \
    --iam-account ${cluster}@${projectId}.iam.gserviceaccount.com
}

echo "
project: $projectId
region: $location
key_ring: $keyRing
crypto_key: $keyName
"

```

