# Application repo for

https://courses.mooc.fi/org/uh-cs/courses/devops-with-kubernetes/chapter-5/gitops

Goal: To use different repositories for the Kubernetes configurations (repo: thomastoumasu/k8s-application) and the code (this repo).  
A push here in code repo triggers a github action here that builds the images and updates the configuration files there with the image names.  
Argo is linked with the configuration repo there and syncs the cluster accordingly.

Added a compose.yaml to be able to deploy here autonomously with docker compose as well, with reverse-proxy for frontend and backend + load balancer in front of the reverse proxy.  
-Just run in repo root:  
docker compose up --scale backend=3 --scale broadcaster=4  
accessible under lb.colasloth.com, see [clone and open locally](https://github.com/thomastoumasu/container-applications-main/blob/main/32Docker%20networking%20-%20MOOC.fi%20courses.pdf)
to check logs: docker compose logs backend --index 3

-Can also just remove the lb and open the reverse-proxy port (e.g., port: 80:80) to have the app accessible under localhost, albeit without scaling possibilities:  
docker-compose build --build-arg VITE_BACKEND_URL=http://localhost:80/api/todos && docker compose up

-Can also deploy this on Google Cloud Compute Engine: provision a (arm C4a) VM with ubuntu (not minimal) and enable http traffic, then SSH:  
sudo -s  
wget --no-check-certificate -O kernel.zip https://github.com/thomastoumasu/k8s-application/archive/main.zip  
unzip kernel.zip  
install docker following https://docs.docker.com/engine/install/ubuntu/  
then:  
docker-compose build --build-arg VITE_BACKEND_URL=<external_ip>:80/api/todos
docker compose up (to access under <external_ip>)
