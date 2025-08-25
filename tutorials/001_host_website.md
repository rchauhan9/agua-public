# Setting up your Website

## Pre-Requisties
- A domain name
- A GCP account
- gcloud CLI installed
- You have some form of frontend webapp to deploy (mine's a Vite React app)

## Steps
1. Create a bucket
2. Upload your website files to the bucket
3. Make the bucket public - required

You may need to run this command if your google account is new. 
This is to provide your account with the permissions to change bucket policies. 
```shell
  gcloud organizations add-iam-policy-binding <project-id> \
      --member=user:<email> \
      --role=roles/orgpolicy.policyAdmin
```

This command allows the objects in the bucket to be publicly viewable.
```shell
  gcloud storage buckets add-iam-policy-binding  gs://<bucket-name> --member=allUsers --role=roles/storage.objectViewer
```

Copy your website files to the bucket.
```shell
    gsutil -m cp -r <local-path-to-website-files>/* gs://<bucket-name>
```

Optionally set a default page and 404 page for bucket website configuration.
```shell
    gsutil web set -m <default-page> -e <404-page> gs://<bucket-name>
```

4. Create a load balancer
5. Add domain records to point to the load balancer IP. Create an A record for your domain pointing to the load balancer's IP address