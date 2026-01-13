# Application repo for

https://courses.mooc.fi/org/uh-cs/courses/devops-with-kubernetes/chapter-5/gitops

Goal: To use different repositories for the Kubernetes configurations (repo: thomastoumasu/k8s-application) and the code (this repo).  
A push here in code repo triggers a github action here that builds the images and updates the configuration files there with the image names.  
Argo is linked with the configuration repo there and syncs the cluster accordingly.

Added a compose.yaml to be able to deploy here autonomously with docker compose as well.
-Just run in repo root:  
docker compose up (to access under localhost)

-Or in Google Cloud Compute Engine, provision a VM with ubuntu and enable http access, then SSH:  
sudo -s  
wget --no-check-certificate -O kernel.zip https://github.com/thomastoumasu/k8s-application/archive/main.zip  
unzip kernel.zip  
install docker following https://docs.docker.com/engine/install/ubuntu/  
then:  
docker-compose build --build-arg VITE_BACKEND_URL=<external_ip>:80/api/todos
docker compose up (to access under <external_ip>)
