# Setting up your App

## Pre-Requisties
- You've followed the steps in the [001 Host Website](../001_gcp_setup.md) tutorial or created the resources from that tutorial
- An example React app to deploy (mine's a Vite React app)

## Steps
1. Create a bucket

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

2. Adjust your load balancer

Create a bucket backend
```shell
  gcloud compute backend-buckets create agua-production-app-backend-bucket \                         
    --gcs-bucket-name=<bucket-name>
```

Add new path matcher to your load balancer for the app
```shell
  gcloud compute url-maps add-path-matcher <load-balancer-name> \                            
    --path-matcher-name=app-matcher --default-backend-bucket=<app-backend-bucket> \
    --new-hosts=<new.host.com>
```

Add new path matcher to your load balancer for the site
```shell
  gcloud compute url-maps add-path-matcher <load-balancer-name> \                            
    --path-matcher-name=site-matcher --default-backend-bucket=<website-backend-bucket> \
    --new-hosts=agua.sh,www.agua.sh
```
3. Update certificate to include new domains

4. Add sub-domain records to point to the load balancer IP. 
Create an A record for your domain pointing to the load balancer's IP address