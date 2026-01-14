# Application repo for

https://courses.mooc.fi/org/uh-cs/courses/devops-with-kubernetes/chapter-5/gitops

Goal: To use different repositories for the Kubernetes configurations (repo: thomastoumasu/k8s-application) and the code (this repo).  
A push here in code repo triggers a github action here that builds the images and updates the configuration files there with the image names.  
Argo is linked with the configuration repo there and syncs the cluster accordingly.

Added a compose.yaml to be able to deploy here autonomously with docker compose as well, with reverse-proxy for frontend and backend + load balancer in front of the reverse proxy.  
Just run in repo root:  
docker compose up --scale backend=3 --scale broadcaster=4  
accessible under lb.colasloth.com, see [clone and open locally](https://github.com/thomastoumasu/container-applications-main/blob/main/32Docker%20networking%20-%20MOOC.fi%20courses.pdf)
to check logs: docker compose logs backend --index 3

(2) Can also rewrite the .yaml with just removing the lb and open the reverse-proxy port (e.g., port: 80:80) to have the app accessible under localhost, albeit without scaling possibilities:  
docker-compose build --build-arg VITE_BACKEND_URL=http://localhost:80/api/todos && docker compose up

Can also deploy this on Google Cloud Compute Engine: provision a arm VM with ubuntu (not minimal), enable http traffic, write www.thomastoumasu.dpdns.org as hostname, (write the allocated IP into the DNS record in Cloudflare), then SSH:  
sudo -s  
wget --no-check-certificate -O kernel.zip https://github.com/thomastoumasu/k8s-application/archive/main.zip  
apt install unzip  
unzip kernel.zip  
install docker with the apt repository following https://docs.docker.com/engine/install/ubuntu/  
then:  
cd k8s-application-main

or in case (2) (can skip writing the hostname in the provisioning above), after installation just do
docker-compose build --build-arg VITE_BACKEND_URL=<external_ip>:80/api/todos
docker compose up (to access under <external_ip>)
