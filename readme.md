# Application repo for

https://courses.mooc.fi/org/uh-cs/courses/devops-with-kubernetes/chapter-5/gitops

Goal: To use different repositories for the Kubernetes configurations (repo: thomastoumasu/k8s-application) and the code (this repo).  
A push here in code repo triggers a github action here that builds the images and updates the configuration files there with the image names.  
Argo is linked with the configuration repo there and syncs the cluster accordingly.

Added a compose.yaml to be able to deploy here autonomously with docker compose as well. Just run in repo root:  
docker compose up
