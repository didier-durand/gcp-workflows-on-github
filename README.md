<img src="https://github.com/didier-durand/gcp-workflows-on-github/blob/master/img/g_cloud.png" height="200">

# Google Cloud SDK workflows on Github CI/CD

![workflow badge](https://github.com/didier-durand/gcp-workflows-on-github/workflows/Create%20GKE%20cluster/badge.svg)
![workflow badge](https://github.com/didier-durand/gcp-workflows-on-github/workflows/Deploy%20image%20on%20Cloud%20Run/badge.svg)
![workflow badge](https://github.com/didier-durand/gcp-workflows-on-github/workflows/Push%20image%20to%20Cloud%20Registry/badge.svg)
![workflow badge](https://github.com/didier-durand/gcp-workflows-on-github/workflows/Cloud%20Build%20for%20Docker%20image/badge.svg)
![workflow badge](https://github.com/didier-durand/gcp-workflows-on-github/workflows/List%20IAM%20roles/badge.svg)
![workflow badge](https://github.com/didier-durand/gcp-workflows-on-github/workflows/List%20project%20services/badge.svg)
![workflow badge](https://github.com/didier-durand/gcp-workflows-on-github/workflows/Use%20GCP%20logging/badge.svg)
![workflow badge](https://github.com/didier-durand/gcp-workflows-on-github/workflows/Use%20GCP%20filestore/badge.svg)
![workflow badge](https://github.com/didier-durand/gcp-workflows-on-github/workflows/Deploy%20to%20GCE/badge.svg)
![workflow badge](https://github.com/didier-durand/gcp-workflows-on-github/workflows/Create%20VPC%20peering/badge.svg)

This repository contains [Github Workflows](https://github.com/features/actions) executing usual scripts for implementation of applications 
on Google Cloud Platform (GCP). They are fully based on CLI commands of the [gcloud SDK](https://cloud.google.com/sdk/install) to allow 
complete automation. To be executable on Github CI/CD environnement, they need to be located in [/.github directory](https://github.com/didier-durand/gcp-workflows-on-github/tree/master/.github/workflows).

All details on all gcloud commands in [gcloud SDK CLI Reference](https://cloud.google.com/sdk/gcloud/reference). On purpose, we use those commands 
with minimum set of options to keep them as neutral as possible for reuse in other projects.

Go to the [Actions tab](https://github.com/didier-durand/gcp-workflows-on-github/actions) to see the results of their last executions reported by the badges 
here above, including the system log messages of Github's Ubuntu runner. They are automatically scheduled on a recurring basis (at least weekly) 
using Github's cron facility.

If you would like to see a new workflow added to this collection, please, open a ticket on this project to ask for it. If you like this project, 
feel free to give it a star to make it more visible to others!

## GCP workflows:

The YAML files below are located in directory [/.github directory](https://github.com/didier-durand/gcp-workflows-on-github/tree/master/.github/workflows):


1. **[create-gke-cluster.yml](https://github.com/didier-durand/gcp-workflows-on-github/blob/master/.github/workflows/create-gke-cluster.yml)**: workflow to test automated deployment of a K8s service on [Google Kubernetes Engine](https://cloud.google.com/kubernetes-engine/docs/concepts/kubernetes-engine-overview). It automatically creates a cluster of 3 nodes, triggers a service deployment based on a public Docker image [tutum/hello-world](https://hub.docker.com/r/tutum/hello-world/)by applying this [YAML description](https://github.com/didier-durand/gcp-workflows-on-github/blob/master/kubernetes/tutum-hello-world.yaml), check its proper functionning (both http & https) and finally deletes the cluster. The service configuration is based on a [Kubernetes external LoadBalancer](https://kubernetes.io/docs/tasks/access-application-cluster/create-external-load-balancer/) in order to obtain an IP address accessible via cURL from anywhere on the Internet.
2. **[deploy-to-gcloud-run.yml](https://github.com/didier-durand/gcp-workflows-on-github/blob/master/.github/workflows/deploy-to-gcloud-run.yml)**: when the fully-managed and serverless compute platform proposed by [Google Cloud Run](https://cloud.google.com/run) (implementation [on GCP](https://cloud.google.com/knative) of Google-sponsored [Knative project](https://knative.dev/)) for deploying and scaling containerized applications quickly and securely provides the right functions, you may want to use this service instead of the full-fledged GKE cluster of the above use case to avoid the management tasks of a Kubernetes cluster. This workflow shows how to build an image on Github, push it to Google Cloud Registry (see next worflow), deploy it to Cloud Run, call the deployed microservice to validate the deployment and finally do the clean-up for Container Registry and Cloud Run.
3. **[push-to-gcr.yml](https://github.com/didier-durand/gcp-workflows-on-github/blob/master/.github/workflows/push-to-gcr.yml)**: workflow to test the build of an image on Github and its push to [Google Container Registry](https://cloud.google.com/container-registry/docs/overview). Then, the image is decribed. Finally, it is deleted for cleanup of test environment.
4. **[gcloud-build.yml](https://github.com/didier-durand/gcp-workflows-on-github/blob/master/.github/workflows/gcloud-build.yml)**: workflow to build a Docker image directly on [Google Build](https://cloud.google.com/cloud-build) (rather than via Github CI/CD if one ) and then push it to GCR. **Tip**: the final step describes the build just done: the id of the build to describe is captured by use of --format(value) in an additional list request.
5. **[gcloud-iam.yml](https://github.com/didier-durand/gcp-workflows-on-github/blob/master/.github/workflows/gcloud-iam.yml)**: workflow to obtain [Identity & Access Management (IAM)](https://cloud.google.com/iam) information: list of service accounts on project, describe a specific service account, list keys attached to a service account.
6. **[gcloud-services.yml](https://github.com/didier-durand/gcp-workflows-on-github/blob/master/.github/workflows/gcloud-services.yml)**: workflow to access resources quotas granted to the project (globally & per region), to list specific GCP services and APIs enabled for the project and then list all services and APIs potentially available to the project. Final step tests the enabling and disabling of a given service.
7. **[gcloud-logging.yml](https://github.com/didier-durand/gcp-workflows-on-github/blob/master/.github/workflows/gcloud-logging.yml)**: workflow to list all active logs of the project and all possible resource descriptors. It also writes a log message in a new test log and reads the last entries produced by all writers. The log aggregation process is asynchronous: a wait is introduced to make sure that the written test log entry may appears in the read step. Finally, the test log is destroyed.
8. **[gcloud-filestore.yml](https://github.com/didier-durand/gcp-workflows-on-github/blob/master/.github/workflows/gcloud-filestore.yml)**: workflow to interact with Filestore service of [Google Cloud Storage](https://cloud.google.com/storage): it lists existing buckets, creates a new bucket, writes files to it, read files from it, lists bucket content and deletes it.
9. **[deploy-to-gce.yml](https://github.com/didier-durand/gcp-workflows-on-github/blob/master/.github/workflows/deploy-to-gce.yml)**: workflow to interact with [Google Compute Engine](https://cloud.google.com/compute): it lists existing GCE compute instances, create a new compute instance, describes it to validate its RUNNING status, stops and finally deletes it.
10. **[gcloud-network.yml](https://github.com/didier-durand/gcp-workflows-on-github/blob/master/.github/workflows/gcloud-network.yml)**: workflow to interact with services of [Google Virtual Private Cloud ](https://cloud.google.com/vpc) (VPC): it creates a couple of networks with each a pair of associateed subnetworks. Those (sub)networks are located in different geographic regions and their CIDR blocks are in the private ranges of [IETF RFC 1918](https://tools.ietf.org/html/rfc1918). So, routing peerings are created between those networks in order to allow communication between workloads deployed in those networks. Those peerings are checked for correctness. Then, cleanup of all components is triggered.

Go to the [Actions tab](https://github.com/didier-durand/gcp-workflows-on-github/actions) to see the results of their last executions. Github does a very good job in protecting the defined secrets: each time the value of ${{ secrets.GCP_PROJECT }} is written in output streams, the Github writer will replace it with ***. Remember it as you read the job executions logs in Actions.

## setup for forks:

When you fork this repository to run it on your own, you will need to recreate two [Github secrets](https://docs.github.com/en/actions/configuring-and-managing-workflows/using-variables-and-secrets-in-a-workflow) in your own repository to get working workflows: 

- ${{ secrets.GCP_PROJECT }} for the GCP project in which you want to execute your tests  
- ${{ secrets.GCP_SA_KEY }} for the secret key of the GCP Service Account(SA) under the behalf of which you want to execute the various GCP commands.

Of course, you need to define the corresponding GCP project and service account as part of your project in your global GCP account so that workflows can properly interact with GCP.

The service account requires following privileges (given via IAM interface) to run the various workflows:

- Service Account User
- Security Reviewer
- Kubernetes Engine Admin
- Cloud Build Editor
- Cloud Build Service Account
- Cloud Run Admin
- Storage Admin
- Storage Object Creator
- Storage Object Viewer
- Logging Admin
- Compute Admin

Also, you need to activate in your GCP project the APIs corresponding to the privileges above in order to use the services. To make things more direct (but less secure...), you can also give the role of Project Owner to your service account in order to obtain all privileges on all resources and services.

Those privileges can probably be tightened - Admin privileges are overkill for some workflows - for productive use of those workflows: minimal security rights are not the priority here.